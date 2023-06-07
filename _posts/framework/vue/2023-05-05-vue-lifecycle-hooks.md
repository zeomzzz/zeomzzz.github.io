---
title:  "Vue Lifecycle Hooks"
excerpt: "Vue Lifecycle Hooks 정리"

categories:
  - Vue.js
tags:
  - [Framework, Vue.js]

toc: true
toc_sticky: true
 
date: 2023-05-05
last_modified_at: 2023-05-05
---

<br>


### **Vue Lifecycle Hooks**

<br>

-   Vue 인스턴스 Lifecycle : 생성(create) → DOM에 부착(mount) → 업데이트(update) → 소멸(destroy)
-   Lifecycle Hook : Vue 인스턴스의 생명주기 각 단계에서 실행되는 함수

<br>


| Lifecycle Hook | 설명 |
| --- | --- |
| `beforeCreate` | \- Vue 인스턴스가 초기화된 직후<br>   \-  `this.$el` 에 접근 불가 ( ∵ DOM에 추가 전)<br>   \- data, methods에 접근 불가 ( ∵ data, event, watcher에 접근하기 전) |
| `created` | \- data를 반응형으로 추적할 수 있음<br>   \- computed, methods, watch 등이 활성화되어 접근 가능<br>   \- `this.$el`에 접근 불가 ( ∵ DOM에 추가 전) |
| `beforeMount` | \- 마운트(DOM에 부착)가 시작되기 전에 호출 <br>  \- 가상 DOM은 생성되었으나, 실제 DOM에 부착하지는 않은 상태 |
| `mounted` | \- 지정된 엘리먼트에 Vue 인스턴스 데이터가 마운트 된 후<br>   \- 가상 DOM의 내용이 실제 DOM에 부착됨 <br>  \- `this.$el`, data, computed, methods, watch 등 모든 요소에 접근 가능 |
| `beforeUpdate` | \- data 값이 바뀌어서 DOM에도 적용시켜야할 때, 변화 직전에 호출 |
| `updated` | \- 변경된 data가 DOM에도 적용된 상태 |
| `beforeDestroy` | \- Vue 인스턴스 제거되기 전에 호출 <br>  \- 해체되기 전이므로 모든 속성에 접근 가능 |
| `destroyed` | \- Vue 인스턴스가 제거된 후 호출 <br>  \- 인스턴스 속성에 접근할 수 없으며, 하위 Vue 인스턴스도 삭제됨 |

<br>
<br>

---

**참고 자료**

-   [Vue.js 가이드](https://v2.ko.vuejs.org/v2/guide/instance.html#%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)
-   [Vue 라이프사이클 이해하기](https://wormwlrm.github.io/2018/12/29/Understanding-Vue-Lifecycle-hooks.html)
