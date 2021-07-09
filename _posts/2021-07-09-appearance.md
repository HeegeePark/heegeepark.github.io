---
layout: post
title: "[iOS] 다크모드에서 상태바 설정"
date: 2021-07-09 14:34:10 +0700
excerpt: xcode light모드 설정
categories: [iOS]
tags: [Issue]
---

## 1️⃣ error

iOS 13 버전 이상부터 다크모드가 제공되면서, 다크모드로 설정되어 있는 모바일 화면에서는 상태바에 있는 내용(LTE, 시간 배터리 등등)이 앱의 배경색이 밝은 경우 잘 안보이는 상황이 존재한다.

<img src="https://user-images.githubusercontent.com/47033052/125012242-d784e000-e0a4-11eb-91e3-e464c77c3083.png" width="50%"/>

## 2️⃣ 해결 방법

`Info.plist`에서 **Appearance** 항목을 추가한 후 이에 대한 value를 **Ligth**로 설정하기.

<img src="https://user-images.githubusercontent.com/47033052/124722328-e81e4480-df44-11eb-936a-d0612713e2d4.png" width="50%"/>

이 앱에서만은 Light모드이기 때문에 상태바의 글자는 검은색으로 나온다.

<img src="https://user-images.githubusercontent.com/47033052/125012243-d784e000-e0a4-11eb-9121-27ddbb2f96fc.png" width="50%"/>
