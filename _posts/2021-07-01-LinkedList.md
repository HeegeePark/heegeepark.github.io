---
layout: post
title: "[Swift] swift에서 연결리스트 구현"
date: 2021-07-01 18:34:10 +0700
excerpt: LinkedList in swift
categories: [Swift]
---

## 1️⃣ Class로 Node 만들기

``` swift
class Node {
  let value: Int	// 노드 값
  let next: Node?		// 다음 노드의 주소를 가짐
  
  init(value: Int, next: Node?) {
    self.value = value
    self.next = next
  }
}
```

## 2️⃣ LinkedList

- `head`는 초기에 nil

``` swift
class LinkedList {
  var head: Node?	// 처음에는 노드가 존재하지 않으므로 옵셔널 타입
  
  func insert(value: Int) {
    guard head != nil else {	// 아무 것도 X: 헤드가 첫 노드 가리키도록
      head = Node(value: value, next: nil)
      return
    }
    var current = head
    while current?.next != nil {	// current가 마지막 노드에 접근할 때까지 이동
      current = current?.next
    }
    currnet?.next = Node(value: value, next: nil)	// current의 next에 새 노드 삽입
  }
  
  func delete(value: Int) {
    if head?.value == value {
      	head = head?.next
      	return
    }
    var previous: Node?		// 이전 노드
    var current = head
    while current != nil && current?.value != value {	// value 찾을 때까지 이동(current가 value인 노드 접근할 때 까지)
			previous = current
      current = current?.next
    }
    previous?.next = current?.next	// 이전 노드의 next에 현재 노드의 다음 노드 저장 (현재 노드 삭제)
  }
  
  func printListItems() {
		var current = head
    while current != nil {
      print(current?.value ?? "")
      current = current?next
    }
  }
}
```
