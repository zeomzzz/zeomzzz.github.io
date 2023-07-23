---
title:  "Linked List"
excerpt: ""

categories:
  - Algorithm
tags:
  - [Algorithm, Linked List]

toc: true
toc_sticky: true
 
date: 2023-07-08
last_modified_at: 2023-07-08
---

<br>

# **Linked List란?**

<br>

- 데이터가 자료의 주소 값으로 서로 연결되어 있는 구조
- 종류
    
    
    | 종류 | 설명 |
    | --- | --- |
    | Singly Linked List<br>(단순 연결 리스트) | - 각 노드에서 단방향으로 연결됨 : 다음 노드로 가는 포인터를 저장<br>- 노드 구성 : 데이터, next(다음 노드) |
    | Doubly Linked List<br>(이중 연결 리스트) | - 각 노드에서 양방향으로 연결됨<br>- 메모리를 추가적으로 사용<br>- 노드 구성 : 데이터, prev, next |
    | Circular Linked List<br>(원형 연결 리스트) | - Singly Linked List와 유사한 단방향 리스트이지만, 맨 앞과 마지막이 연결됨<br>- 한 노드에서 모든 노드로 접근 가능 |
- 시간 복잡도
    
    
    | 동작 | 시간 복잡도 |
    | --- | --- |
    | 인덱스로 조회 | O(N) |
    | 삽입 | O(1) |
    | 삭제 | O(N) |

<br>
<br>

## **Array vs. Linked List**

<br>

| 종류 | Array | Linked List |
| --- | --- | --- |
| 장점 | - 무작위 접근 가능(메모리에 연속적으로 존재하기 때문) | - 삽입, 삭제 빠름<br>- 자유로운 크기 조절 |
| 단점 | -  삽입, 삭제 느림<br>- 크기 조절 불가능 | - 순차 접근만 가능 |

<br>
<br>

## **Linked List 메소드**

<br>

- java
    
    ```java
    import java.util.LinkedList;
    
    public class Test {
    	public static void main(String[] args) {
    		LinkedList<Integer> list = new LinkedList<>();
    		
    		/* 추가 */
    		// 가장 뒤에 추가
    		list.add(n);
    		list.addLast(n);
    		// 가장 앞에 추가
    		list.addFirst(n);
    		list.push(n);
    		// 가장 뒤에 추가
    		list.offer(n);
    		// 특정 인덱스에 추가
    		list.add(idx, n);
    
    		/* 제거 */
    		// 특정 인덱스의 원소를 제거
    		list.remove(idx);
    		// 가장 앞에 있는 원소를 제거
    		list.remove();
    		list.removeFirst();
    		// 가장 뒤에 있는 원소를 제거
    		list.removeLast();
    		// 가장 앞에 있는 요소 튀어나옴
    		list.pop();
    		list.poll();
    		
    		/* 그 외 */
    		// 현재 리스트의 원소의 개수
    		list.size();
    		// 리스트 비어있는지 여부
    		list.isEmpty();
    
    	}
    }
    ```

<br> 
<br>

# **Linked List 구현**

<br>

```java
class LinkedList {
	Node header;

	static class Node {
		int data;
		Node next = null;
	}

	LinkedList() {
		header = new Node();
	}

	void insert(int d) {
		Node end = new Node(d);
		Node n = this; // 첫번째 노드를 선택
		
		while(n.next != null) {
			n = n.next;
		} // n에 마지막 노드가 들어오면 while 종료
	
		n.next = end;
	
	}
	
	void delete(int d) {
		Node n = header;
		while(n.next != null) {
			if(n.next.data == d) {
				n.next = n.next.next;
			} else {
				n = n.next;
			}
		}
	}
	
	void retrieve() {
		Node n = header.next;
		while (n.next != null) {
			System.out.print(n.data + " -> ");
			n = n.next;
		}
		System.out.println(n.data);
	}

}

// test
public class LinkedListNode {
	public static void main (String[] args) {
		LinkedList lst = new LinkedList();
		// test code 작성 및 test
	}
}
```

<br>

---

**참고자료**

<br>

- [[자료구조 알고리즘] LinkedListNode의 구현 in Java](https://youtu.be/IrXYr7T8u_s)