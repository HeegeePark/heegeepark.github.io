---
layout: post
title: "[iOS] UIControl.Event"
date: 2021-08-10 20:34:10 +0700
excerpt: 도큐먼트로 공부하는 UIControl
categories: [iOS]
tags: [Documentation]
---

> 참조
>
> - [부스트코스 - 인터페이스 빌더의 객체를 코드와 연결(IBAction)](https://www.boostcourse.org/mo326/lecture/16847?isDesc=false)
> - [애플 공식 문서 - UIControl.Event](https://developer.apple.com/documentation/uikit/uicontrol/event)

## 1️⃣ 컨트롤 이벤트와 액션과의 관계

UIKit에는 UIButton, UISwitch, UIStepper 등 UIControl을 상속받은 다양한 컨트롤 클래스가 있음.

그런 컨트롤 객체에 발생한 다양한 이벤트 종류를 특정 액션 메서드에 연결할 수 있음.

➡️ 컨트롤 객체에서 특정 이벤트 발생하면, 미리 지정해 둔 타겟의 액션 호출됨.

## 2️⃣ 컨트롤 이벤트(UIControl.Event)의 종류

컨트롤 이벤트는 UIControl에 Event라는 타입으로 정의되어 있음

``` swift
struct Event
```

### touchDown

컨트롤을 터치했을 때 발생하는 이벤트

### touchDownRepeat

컨트롤을 연속 터치할 때 발생하는 이벤트

- UITouch tapCount 메서드 값이 1보다 큼

### touchDragInside

컨트롤 범위 내에서 너치한 영역을 드래그할 때 발생하는 이벤트

### touchDragOutSide

터치 영역이 컨트롤 바깥 쪽에서 드래그할 때 발생하는 이벤트

### touchDragEnter

터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔다가 다시 들어왔을 때 발생하는 이벤트

### touchDragExit

터치 영역이 컨트롤의 일정 영역 바깥쪽으로 나갔을 때 발생하는 이벤트

### touchUpInside

컨트롤 영역 안쪽에서 터치 후 뗐을 때 발생하는 이벶트

### touchUpOutside

컨트롤 영역 안쪽에서 터치 후 컨트롤 밖에서 뗐을 때 이벤트

### touchCancel

터치를 취소하는 이벤트(touchUp 이벤트가 발생하지 않음)

### valueChanged

터치를 드래그 및 다른 방법으로 조작하여 값이 변경되었을 때 발생하는 이벤트

### primaryActionTriggered

버튼이 눌릴 때 발생하는 이벤트(iOS보다는 tvOS에서 사용)

### editingDidBegin

UITextField에서 편집이 시작될 때 호출되는 이벤트

### editingChanged

UITextField에서 값이 바뀔 때마다 호출되는 이벤트

### editingDidEnd

UITextField에서 외부객체와의 상호작용으로 인해 편집이 종료되었을 때 발생하는 이벤트

### editingDidEndOnExit

UITextField에서 편집상태에서 키보드의 return 키를 터치했을 때 발생하는 이벤트

### allTouchEvents

모든 터치 이벤트

### allEditingEvents

UITextField에서 편집작업의 이벤트

### applicationReserved

각각의 애플리케이션에서 프로그래머가 임의로 지정할 수 있는 이벤트 값 범위

### systemReserved

프레임워크 내에서 사용하는 예약된 이벤트 값의 범위

### allEvents

시스템 이벤트를 포함한 모든 이벤트

