---
title:  "Kotlin으로 구글 로그인"
excerpt: ""

categories:
  - Web
tags:
  - [Auth, GAuth, Social Login]

toc: true
toc_sticky: true
 
date: 2023-10-24
last_modified_at: 2023-10-24
---

<br>

<div class="descipt-font">
안드로이드 앱 프로젝트를 진행하며 로그인 및 회원가입 API를 개발하기 전에 클라이언트에서 받을 수 있는 반환값을 확인하기 위하여 Kotlin을 이용해 구글 로그인을 구현해보았다. <br>
xml과 kotlin 코드가 분리되어있고, gradle 빌드를 하는 점에서 프론트엔드 프레임워크인 Vue.js나 React보다는 백엔드 프레임워크인 SpringBoot와 비슷하게 느껴졌다. 
</div>

<br>

## **0. 사전 작업**

### **1) Android 프로젝트에 Firebase 추가**

- 안드로이드 스튜디오에서 프로젝트 생성
- [Firebase Console](https://console.firebase.google.com/u/1/?hl=ko&_gl=1*rei9yx*_ga*MTIxMTM1MDI4OC4xNjk3MDc3NDA1*_ga_CW55HF8NVT*MTY5ODAzMTI0Ni4xMi4xLjE2OTgwMzEzMDEuNS4wLjA.)에 안드로이드 앱 등록
    - [SHA-1 등록](https://jgeun97.tistory.com/203) : 터미널에 `./gradlew signingReport`
    - app 루트 폴더에 google-services.json 추가
    - Firebase SDK 추가
        - 프로젝트 수준의 build.gradle.kts에 [Google 서비스 Gradle 플러그인](https://developers.google.com/android/guides/google-services-plugin?hl=ko) 추가
            - Firebase SDK가 google-services.json 구성 파일의 값에 액세스할 수 있도록
            
            ```kotlin
            plugins {
                // ...
                id("com.google.gms.google-services") version "4.4.0" apply false
            		// ...
            }
            ```
            
        - 모듈(앱) 수준 build.gradle.kts에 google-services 플러그인과 앱에서 사용할 Firebase SDK 추가
            
            ```kotlin
            plugins {
              id("com.android.application")
            
              // Add the Google services Gradle plugin
              id("com.google.gms.google-services")
              ...
            }
            
            dependencies {
              // Import the Firebase BoM
              implementation(platform("com.google.firebase:firebase-bom:32.4.0"))
            
              // TODO: Add the dependencies for Firebase products you want to use
              // When using the BoM, don't specify versions in Firebase dependencies
              implementation("com.google.firebase:firebase-analytics-ktx")
            
              // Add the dependencies for any other desired Firebase products
              // https://firebase.google.com/docs/android/setup#available-libraries
            }
            ```

<br>      

### **2) 모듈(앱 수준) Gradle 파일에 라이브러리 추가**

- 라이브러리 버전 관리 제어 : [Firebase Android BoM](https://firebase.google.com/docs/android/learn-more?hl=ko#bom)
- Firebase 인증을 설정하는 과정에서 앱에 Google Play 서비스 SDK를 추가 필요

```kotlin
dependencies {
    // Import the BoM for the Firebase platform
    implementation(platform("com.google.firebase:firebase-bom:32.3.1"))

    // Add the dependency for the Firebase Authentication library
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation("com.google.firebase:firebase-auth-ktx")

    // Also add the dependency for the Google Play services library and specify its version
    implementation("com.google.android.gms:play-services-auth:20.7.0")
}
```

<br>

### **3) SHA 디지털 지문 지정하지 않았다면 설정**

<br>

### **4) Firebase Console에서 Google을 로그인 방법으로 사용 설정**

- Firebase Console에서 인증 섹션 접속
- 로그인 방법 > Google 로그인 방법 사용 설정 > 저장
- 업데이트된 Firebase 구성 파일(google-services.json)을 다운로드 > 안드로이드 스튜디오 프로젝트에서 구성 파일을 교체

<br>
<br>

## **1. Firebase에 인증**

### **1) Firebase 이용하여 Google Login**

- 서버 클라이언트 ID
    - '서버' 클라이언트 ID(`your_web_client_id`) 확인 방법
        - [Google Cloud의 사용자 인증 정보 페이지](https://console.cloud.google.com/apis/credentials?hl=ko) 접속 > 웹 애플리케이션 유형의 클라이언트 ID가 백엔드 서버의 OAuth 2.0 클라이언트 ID
    - 안드로이드 스튜디오의 New String Value Resource 메뉴에서 지정
- [Firebase로 사용자 정보 확인하기](https://firebase.google.com/docs/auth/android/manage-users?hl=ko)
- [구현 코드](https://github.com/zeomzzz/social-auth-practice/tree/main/google/google_front_kotlin3) ([참고자료](https://onlyfor-me-blog.tistory.com/480))
    - MainActivity.kt
        
        ```kotlin
        package com.zeomzzz.google_front_kotlin3
        
        import android.content.Intent
        import android.os.Bundle
        import android.util.Log
        import androidx.activity.result.ActivityResultCallback
        import androidx.activity.result.ActivityResultLauncher
        import androidx.activity.result.contract.ActivityResultContracts
        import androidx.appcompat.app.AppCompatActivity
        import androidx.databinding.DataBindingUtil
        import com.google.android.gms.auth.api.signin.GoogleSignIn
        import com.google.android.gms.auth.api.signin.GoogleSignInOptions
        import com.google.android.gms.common.api.ApiException
        import com.google.firebase.auth.AuthCredential
        import com.google.firebase.auth.FirebaseAuth
        import com.google.firebase.auth.FirebaseUser
        import com.google.firebase.auth.GoogleAuthProvider
        import com.zeomzzz.google_front_kotlin3.databinding.ActivityMainBinding
        import kotlinx.coroutines.CoroutineScope
        import kotlinx.coroutines.Dispatchers
        import kotlinx.coroutines.launch
        
        class MainActivity : AppCompatActivity() {
        
            private lateinit var binding: ActivityMainBinding
            private val TAG = this.javaClass.simpleName
        
            private lateinit var launcher: ActivityResultLauncher<Intent>
            private lateinit var firebaseAuth: FirebaseAuth
        
            private var email: String = ""
            private var tokenId: String? = null
            private var uid: String = ""
        
            override fun onCreate(savedInstanceState: Bundle?) {
        
                Log.e(TAG, "onCreate started")
        
                super.onCreate(savedInstanceState)
                binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        
                firebaseAuth = FirebaseAuth.getInstance()
                launcher = registerForActivityResult(
                    ActivityResultContracts.StartActivityForResult(), ActivityResultCallback { result ->
                        Log.e(TAG, "resultCode : ${result.resultCode}")
                        Log.e(TAG, "result : $result")
                        if (result.resultCode == RESULT_OK) {
                            val task = GoogleSignIn.getSignedInAccountFromIntent(result.data)
                            try {
                                task.getResult(ApiException::class.java)?.let { account ->
                                    tokenId = account.idToken
                                    Log.e(TAG, "tokenId : $tokenId")
                                    if (tokenId != null && tokenId != "") {
                                        val credential: AuthCredential = GoogleAuthProvider.getCredential(account.idToken, null)
                                        firebaseAuth.signInWithCredential(credential)
                                            .addOnCompleteListener {
                                                if (firebaseAuth.currentUser != null) {
                                                    val user: FirebaseUser = firebaseAuth.currentUser!!
                                                    email = user.email.toString()
                                                    uid = user.uid
        
                                                    Log.e(TAG, "email : $email")
                                                    Log.e(TAG, "uid : $uid")
                                                    val googleSignInToken = account.idToken ?: ""
                                                    if (googleSignInToken != "") {
                                                        Log.e(TAG, "googleSignInToken : $googleSignInToken")
                                                    } else {
                                                        Log.e(TAG, "googleSignInToken이 null")
                                                    }
                                                }
                                            }
                                    }
                                } ?: throw Exception()
                            }   catch (e: Exception) {
                                e.printStackTrace()
                            }
                        }
                    })
        
                binding.run {
                    googleLoginButton.setOnClickListener {
                        CoroutineScope(Dispatchers.IO).launch {
                            val gso = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                                .requestIdToken(getString(R.string.default_web_client_id))
                                .requestEmail()
                                .build()
                            val googleSignInClient = GoogleSignIn.getClient(this@MainActivity, gso)
                            val signInIntent: Intent = googleSignInClient.signInIntent
                            launcher.launch(signInIntent)
                        }
                        Log.e(TAG, "로그인 합니다.")
                    }
        
                    googleLogoutButton.setOnClickListener {
                        CoroutineScope(Dispatchers.IO).launch {
                            val gso = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                                .requestIdToken(getString(R.string.default_web_client_id))
                                .requestEmail()
                                .build()
                            val googleSignInClient = GoogleSignIn.getClient(this@MainActivity, gso)
        
                            googleSignInClient.signOut()
                            Log.e(TAG, "로그아웃 합니다.")
                        }
                    }
                }
            }
        }
        ```
        
    - activity_main.xml
        
        ```kotlin
        <?xml version="1.0" encoding="utf-8"?>
        <layout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools">
        
            <androidx.constraintlayout.widget.ConstraintLayout
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                tools:context=".MainActivity">
        
                <Button
                    android:id="@+id/google_login_button"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="구글 로그인"
                    app:layout_constraintBottom_toBottomOf="parent"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toTopOf="parent"
                    app:layout_constraintVertical_bias="0.405" />
        
                <Button
                    android:id="@+id/google_logout_button"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="구글 로그아웃"
                    app:layout_constraintBottom_toBottomOf="parent"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintHorizontal_bias="0.498"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toTopOf="parent"
                    app:layout_constraintVertical_bias="0.547" />
        
            </androidx.constraintlayout.widget.ConstraintLayout>
        
        </layout>
        ```
        
<br>

> 🚨 **AndroidManifest.xml에서 MainActivity.kt 인식 못하는 오류**
>
> - 앱을 실행하자 마자 종료됨
> - 해결 방법 : MainActivity.kt에 package 이름 추가

<br>

---

**참고 문서**

<br>

1) 공식 문서

- [Android에서 Google로 인증](https://firebase.google.com/docs/auth/android/google-signin?hl=ko)
- [Android 프로젝트에 Firebase 추가](https://firebase.google.com/docs/android/setup?hl=ko#add-config-file)
- [Google 서비스 Gradle 플러그인](https://developers.google.com/android/guides/google-services-plugin?hl=ko)
- [GoogleSignInActivity 예제 (firebase 공식)](https://github.com/firebase/snippets-android/blob/master/auth/app/src/main/java/com/google/firebase/quickstart/auth/kotlin/GoogleSignInActivity.kt)
- [Firebase에서 사용자 관리하기](https://firebase.google.com/docs/auth/android/manage-users?hl=ko)

<br>

2) 그 외

- [[Android] SHA-1 key 쉽게 확인하기](https://jgeun97.tistory.com/203)
- [[Android] 코틀린으로 구글 로그인 구현하기](https://onlyfor-me-blog.tistory.com/480)

<br>
<br>