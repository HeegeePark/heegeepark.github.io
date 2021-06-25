---
layout: post
title: "[Swift] swift에서 문자열 자르기"
date: 2021-05-08 18:34:10 +0700
excerpt: index∙substring 정복!
categories: [Swift]
---

### 참고

-  https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html
- http://www.appleofeyes.com/%EB%AC%B8%EC%9E%90%EC%97%B4-%EC%9E%90%EB%A5%B4%EA%B8%B0-substring-swift-3-0/

## Index 관련 value

- `startIndex` : 첫번째 문자의 인덱스
- `endIndex` : 마지막 문자 다음의 인덱스

### 예제

``` swift
var str = "Hello, Swift"
str[str.startIndex]		// H
str[str.endIndex]		// error: after last character (t 다음 인덱스)

// range 
let range = str.startIndex..<str.endIndex
str[range]	// "Hello, Swift"
```



## after

- `after` : 입력된 인덱스의 바로 다음 인덱스

### 예제

``` swift
let secondIndex = str.index(after: str.startIndex)
str[secondIndex]		// e
```



## before

- `before` : 입력된 인덱스의 바로 이전 인덱스

### 예제

``` swift
let lastIndex = str.index(before: str.endIndex)
str[lastIndex]		// t
```



## offsetBy

- `offsetBy`: 입력된 인덱스와의 거리값
  - `Int` 입력 가능

### 예제

``` swift
let sevenIndex = str.index(str.startIndex, offsetBy: 7)
str[sevenIndex]		// S (7번째 인덱스)
```



## Substring(to: String.Index)

- 처음 ~ to 인덱스까지 자르기

``` swift
let index = str.index(str.startIndex, offsetBy: 5)
str.substring(to: index)  // Hello
```



## Substring(from: String.Index)

- from 인덱스 ~ 끝까지 자르기

``` swift
let index = str.index(str.startIndex, offsetBy: 7)
str.substring(from: index)  // Swift
```



## Substring(with: Range)

- 설정 구간의 값 가져오기

``` swift
let start = str.index(str.startIndex, offsetBy: 7)
let end = str.index(str.endIndex, offsetBy: -6)
let range = start..<end
str.substring(with: range)  // Swif
```

