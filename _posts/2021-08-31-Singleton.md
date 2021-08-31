---
layout: post
title: "[iOS] Swift 디자인 패턴 Singleton"
date: 2021-08-31 20:34:10 +0700
excerpt: Swift로 구현하는 디자인 패턴
categories: [iOS]
tags: [Swift, Architecture]

---

> 참조
>
> - [블로그](https://babbab2.tistory.com/66)

## 1️⃣ Singleton Pattern이란?

특정 용도로 객체를 하나만 생성하여, 공용으로 사용하고 싶을 때 사용하는 디자인 유형

객체가 앱에서 유일하게 하나만 존재하여 다른 객체들 그 안에 내용을 공유하는 방식!

- 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.

``` swift
class Singleton {
		static let shared = Singleton()  // static 변수로 자신을 반환
		private init() {}  // private이기 때문에 다른 곳에서 new 할 수 없음
}
```

- 해당 클래스와 같은 타입의 타입 프로퍼티를 생성하여 객체를 생성하지 않아도 접근이 가능하도록 함.
- static 전역 변수로 선언하는데 이 프로퍼티는 지연생성(lazy)되기 때문에 처음 Singleton Class를 생성하기 전 까지는 메모리에 올라가지 않음.
- 생성자를 prvate으로 선언해줌으로써 여러 개의 객체 생성을 방지함.

## 2️⃣ 싱글톤 패턴을 적용한 예제

### 1️⃣ 싱글톤 클래스 형태의 UserInfo

static을 이용해 Instance를 저장할 프로퍼티 **shared** 생성해주고, private으로 init 함수 접근 제어하기

``` swift
class UserInfo {
    static let shared = UserInfo()

    var id: String?
    var password: String?
    var name: String?
  
  	private init() { }
}
```



### 2️⃣ 싱글톤 클래스 접근하기

위에서 생성한 static 프로퍼티를 이용하면 끝!

Instance를 뷰컨트롤러마다 생성해주는 것 같아 보여도, 하나의 Instance를 공유하고 있음.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVmsQc%2FbtqOYt0xgaU%2Fk4fR7SVzSexrukeToKNAKk%2Fimg.png" width="50%" />

``` swift
// A ViewController
let userInfo = UserInfo.shared
userInfo.id = "Sodeul"
```

``` swift
// B ViewController
let userInfo = UserInfo.shared
userInfo.password = "123"
```

``` swift
// C ViewController
let userInfo = UserInfo.shared
userInfo.name = "Sodeul"
```

## 3️⃣ Swift 싱글톤의 장점

Singleton을 생성하는 것은 **Multi Threading 환경에서 Thread-Safe하지 않기 때문**에 **여러 쓰레드**가 만약 **동시에 Singleton을 생성**하면,

경우에 따라 **Instance가 2개 3개 생성될 수도 있다** 

하지만, Swift에서는

static을 사용해 타입 프로퍼티로 인스턴스를 생성하면, **사용 시점에 초기화(lazy)** 되기 때문에 **Singleton Instance가 최초 생성되기 전까진** **메모리에 올라가지 않고,** **Dispatch_once도 자동 적용**되어 **Thread-Safe**하게 사용할 수 있다.
