---
layout: post
title: "[알고리즘] 리트코드 Longest Palindromic Substring swift 풀이"
date: 2022-05-16 14:34:10 +0700
excerpt: [DP] 거꾸로 해도 똑같은(팰린드롬) 가장 긴 부분 문자열 찾기
categories: [Algorithm]
tags: [Algorithm, DP]
---

## **1️⃣ 문제**

> [문제 - 5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
>
> [풀이 참조 - Youtube](https://www.youtube.com/watch?v=byWNJiBvHXc)

## 2️⃣ 풀이 방법 및 코드

### 1️⃣ 로직

투 포인터 left, right를 이용하여 특정 인덱스에서 한칸씩 늘려서 회문(팰린드롬)인지 확인하는 방법

<img src="https://blog.kakaocdn.net/dn/cIXOvI/btqVTHFydFk/6oGASCcSl5gbkSWeX4HOQ1/img.png" width="538" />

### 2️⃣ 소스 코드

입력으로 주어진 문자열(s)의 각 인덱스마다 expand() 함수에서 양 끝 포인터가 가리키는 두 문자가 서로 같을 때까지 늘려주어 최대 길이를 찾을 때마다 answer에 저장해준다.

``` swift
class Solution {
    // 부분 문자열 확장하는 함수
    func expand(_ left: Int, _ right: Int, _ s: [String]) -> String {
        var newLeft = left
        var newRight = right
        while newLeft >= 0 && newRight < s.count && s[newLeft] == s[newRight] {
            newLeft -= 1
            newRight += 1
        }
        return s[newLeft + 1..<newRight].joined()
    }
    
    func longestPalindrome(_ s: String) -> String {
        var sArr = s.map { String($0) }
        var answer = ""
        
        if s.count < 2 || s == sArr.last! { return s }
        
        for i in 0..<s.count-1 {
            answer = [answer, expand(i, i, sArr), expand(i, i+1, sArr)].max { $0.count < $1.count }!
        }        
        return answer
    }
}
```



## 3️⃣ 이 문제를 풀면서 되새긴 점

### [max(by:)](https://developer.apple.com/documentation/swift/array/2294243-max)

<img width="538" alt="image" src="https://user-images.githubusercontent.com/47033052/168562339-dbad89dd-61c6-4c9c-a80b-a50ad0757421.png">

Closure를 이용하여 원하는 max 기준에 충족하는 값 찾기

- Optional이므로 ! 붙여주기

```swift
let hues = ["Heliotrope": 296, "Coral": 16, "Aquamarine": 156]
let greatestHue = hues.max { a, b in a.value < b.value }
print(greatestHue)
// Prints "Optional((key: "Heliotrope", value: 296))"
```