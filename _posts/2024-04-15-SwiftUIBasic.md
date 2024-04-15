---
layout: post
title: "[iOS] SwiftUI 구조 이해하기"
date: 2024-04-15 19:34:10 +0700
excerpt: SwiftUI 개발한다면 꼭 알아야 할 것
categories: [iOS]
tags: [iOS, SwiftUI]
---

> 참조
> 새싹 강의자료
> [SwiftUI — some View에서 some은 뭐고, 왜 필요할까?](https://minsson.medium.com/swiftui-some-view%EC%97%90%EC%84%9C-some%EC%9D%80-%EB%AD%90%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C-67c25f5452c)

## 0️⃣ SwiftUI란?

Swift UI는 UIKit 위에서 빌드되는 프레임워크로, 선언형 방식의 구조를 가지고 있음.

- **선언적 구문(declairative syntax)**으로 단순하면서 직관적인 구문을 이용하여 화면 기술 가능

목적: 앱 개발을 더 쉽고 빠르게, 소프트웨어 개발 시 발생하는 버그를 줄이기 위해

### UIkit과의 차이점

- 스유 하나를 알면 다양한 애플 플랫폼 개발 가능

- 유킷은 클래스 기반, 스유는 값 타입인 구조체 기반

- 유킷은 오토레이아웃으로 레이아웃으로 조절하는 반면, 스유는 레이아웃 메니지 종류(VStack, HStack, Form, List)들로 명시한 후, 수정자(modifier)로 속성을 설정함.

  ➡️ 선언된 내용을 통해 스유가 자동으로 constraints, 렌더링 등 복잡한 세부 사항을 자동으로 처리함.

  

## 1️⃣ 프로젝트 구조

유킷의 AppDelegate, SceneDelegate가 없는 대신, 프로젝트명과 동일한 이름의 파일이 있는데 역할을 대신함.

여기서의 ContentView는 구조체이므로 View라는 프로토콜을 준수하는 것을 알 수 있음. 

<img width="244" alt="image" src="https://github.com/HeegeePark/SeSAC/assets/47033052/d45a3382-0d4b-4a9f-9aa8-1a8b5549e4b2">



## 2️⃣ body 앞의 some은 무엇이고, 왜 some View라고 표기할까?

`some`은 Opaque type을 나타내는 키워드

- 리턴 타입을 자동으로 그리고 빠르게 추론할 수 있는 스위치 기능을 제공함.

- 함수 외부에서 반환값의 타입을 정확히 알지 못하게 숨길 수 있음.

### 그렇다면 왜 숨겨야하나?

- 사실 해당 뷰의 타입은 VStack<TupleView<(Text, Text)>> 이다. 해당 뷰의 타입을 구체적으로 입력할 수 있으나, 뷰의 구성이 변경될 때마다 이 타입을 계속 업데이트해야하는 번거로움 발생.
- some 키워드를 사용해 View를 선언하고 컴파일 타임에 구체적인 타입을 결정하는 역제네릭 하는 것이 생산성에 굿굿

``` swift
var body: some View {
   VStack(alignment: .leading) {
       Text("My hovercraft is full of eels")
           .font(.headline)
       Text("Mijn luchtkussenboot zit vol paling")
           .font(.subheadline)
   }
}
```



## 3️⃣ Opaque Type

- Swift 5.1+

- 불투명한 타입
- 역제네릭 타입
  - 구현부에서는 타입을 알지만, 함수 사용 등 값을 사용하는 외부에서는 타입을 숨김.
- 프로토콜을 쓰면서 앞에 some 키워드를 붙이면, ‘정확히 뭔지는 불명확하지만, 아무튼 이 프로토콜을 준수하는 어떤 타입 중에 하나일 것은 분명하다’라는 뜻



## 4️⃣ Modifier

View의 속성이 변경될 때마다 body의 타입이 변경되는데, 주범으로 Modifier가 있음.

- 뷰의 속성은 modifier method를 사용하여 설정함.
- modifier를 사용하면 속성이 적용된 새로운 뷰를 반환하기 때문에 타입이 변경되는 것.
- 각 modifier가 적용된 뷰가 매번 새롭게 반환되기 때문에, modifier 적용 순서에 따라 다른 결과를 얻을 수 있음.



## 5️⃣ body 프로퍼티에서 변수를 바로 수정할 수 없는 이유

가변 프로퍼티라도 값 변경이 안됨. 이유는??

- body는 구조체이고, 구조체의 연산 프로퍼티의 getter 속성은 `nonmutationg`이기 때문!

<img src="https://github.com/HeegeePark/SeSAC/assets/47033052/b2f8c514-ca03-46e7-aa46-1f3184a9ad97" alt="image" style="zoom: 50%;" />

### 그렇다면, mutating getter로 변경하면 해결가능?

일반적인 구조체는 해결 가능하지만, body는 해결 불가!

View 프로토콜은 body프로퍼티를 가지고 있어야하는데, 없다는 에러 발생

- body가 있는데 없다고 뜨는 이유
  - View 프로토콜 내 규약된 body는 mutating get으로 선언되어 있지 않아, body를 못찾는 것임.

<img src="https://github.com/HeegeePark/SeSAC/assets/47033052/410efc25-0415-4b76-9732-cd9766d10be6" alt="image" style="zoom: 50%;" />

### body의 프로퍼티 값 변경 해결 방법

View의 상태를 저장하고 수정하는 방식인 `@State` 또는 `@Binding`으로 해결

<img src="https://github.com/HeegeePark/SeSAC/assets/47033052/549983c1-846d-43b3-9bdc-1068e0eca0dc" alt="image" style="zoom:25%;" />
