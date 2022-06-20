---
layout: post
title: "[알고리즘] 프로그래머스 JadenCase 문자열 만들기 swift 풀이"
date: 2022-06-12 14:34:10 +0700
excerpt: 문자열 처리 문제
categories: [Algorithm]
tags: [Algorithm, 문자열]
---

## **1️⃣ 문제**

> [문제 - JadenCase 문자열 만들기](https://programmers.co.kr/learn/courses/30/lessons/12951)

###### 문제 설명

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

##### 제한 조건

- s는 길이 1 이상 200 이하인 문자열입니다.
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
  - 숫자는 단어의 첫 문자로만 나옵니다.
  - 숫자로만 이루어진 단어는 없습니다.
  - 공백문자가 연속해서 나올 수 있습니다.

##### 입출력 예

| s                       |         return          |
| ----------------------- | :---------------------: |
| "3people unFollowed me" | "3people Unfollowed Me" |
| "for the last week"     |   "For The Last Week"   |



## 2️⃣ 풀이 방법 및 코드

### 1️⃣ 처음 풀이한 코드

공개된 테케는 통과했지만, 제출하면 몇몇 케이스들이 "시간 초과" ㅠ

아무래도 연속된 공백을 while문으로 계속 찾는게 원인이지 않을까(?)

``` swift
import Foundation

extension String {
    func removeContinuousBlank() -> String {
        var newS = self
        while newS.contains("  ") {
            newS.replacingOccurrences(of: "  ", with: " ")
        }
        return newS
    }
}

func solution(_ s:String) -> String {
    var str = s.removeContinuousBlank().map { String($0) }
    let start = str.first!
    var answer = start.uppercased()
    
    if s.count == 1 { return answer }
    var idx = 1
    while idx < str.count {
        if str[idx] == " " && idx + 1 < str.count {
            answer += " " + str[idx+1].uppercased()
            idx += 2
        } else {
            answer += str[idx].lowercased()
            idx += 1
        }
    }
    return answer
}
```



### 2️⃣ 두번째로 고친 코드

`joined(separator: " ")`도 구분자와 함께 join할 수 있다는 점을 알게 되어, 

이전의 완전 탐색 방법이 아닌, 공백 기준 단어를 끊고, 각 단어들을 JadenCase처리한 후 공백 기준으로 다시 String으로 붙여주었다.

**그러나,** 내가 간과한 점이 하나 있다.

**"연속된 공백도 다시 return에서 연속된 공백으로 나와야 한다(공백이 하나로 change X)"**

``` swift
import Foundation

func solution(_ s:String) -> String {
    return s.split(separator: " ").map { 
        var word = String($0)
        var prefix = String(word.removeFirst())
        return prefix.uppercased() + word.lowercased()
    }.joined(separator: " ")
}
```

### 3️⃣ 최종 코드

아래 되새긴 점에 언급한 것처럼, `components(separatedBy: " ")` 구문으로 문자열을 자르면,

공백과 공백의 사이에도 ""라는 원소가 존재한다고 가정하여, 자른 문자열에 포함시킨다.

(split()은 연속된 ""는 패스함)

따라서, `components(separatedBy: " ")`를 이용하여 연속된 공백의 개수를 유지하여 다시 문자열을 join할 수 있다.

``` swift
import Foundation

func solution(_ s:String) -> String {
    return s.components(separatedBy: " ").map { 
        guard !$0.isEmpty else { return "" }
        var word = String($0)
        var prefix = String(word.removeFirst())
        return prefix.uppercased() + word.lowercased()
    }.joined(separator: " ")
}
```



## 3️⃣ 이 문제를 풀면서 되새긴 점

### .split(separator: " ")과 .components(separatedBy: " ")의 찐 차이점

기존에는 결과는 동일하나 return type만 다른 줄 알았는데, 연속된 공백이 있을 시 결과도 다르다!

```swift
let arr = "ga   na"	// 3개의 연속된 공백

print(arr.split(separator: " ").map { String($0) })
// ["ga", "na"]
print(arr.components(separatedBy: " "))
// ["ga", "", "", "na"]
```

