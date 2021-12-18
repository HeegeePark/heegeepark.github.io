---
layout: post
title: "[알고리즘] 프로그래머스 실패율 swift 풀이"
date: 2021-12-03 16:34:10 +0700
excerpt: 2019 카카오 블라인드 기출
categories: [Algorithm]
tags: [Algorithm, Kakao, 정렬]
---

## 1️⃣ 문제

> [문제 - 카카오 거리두기 확인하기](https://programmers.co.kr/learn/courses/30/lessons/42889?language=swift)
>
> [풀이 참조 - Blog](https://keeplo.tistory.com/160)

~~(시험기간이지만 갑자기 1일 1문제는 풀어야하지 않겠나며 레벨1후딱 풀고 시험 공부하자!하면서 호기롭게 시작했는데, 생각보다 버벅댔다,, 역시 알고리즘은 미리미리 익혀야지 감 잃었으,,)~~

실패율만 구해서 정렬하기만 하면 되므로 쉬운 문제로 보이지만, 한 가지 고려해야 할 점이 있다. 자세한 내용은 다음 풀이 방법으로 ( ͡o ͜ʖ ͡o)

## 2️⃣ 풀이 방법 및 코드

### 1️⃣ 처음 생각하고 구현한 방법

처음에는 아래와 같은 `getFailureRate`라는 실패율을 구하는 함수로 스테이지별 실패율을 저장해주고, 내림차순 정렬하여 return 하는 방법을 이용하였으나, 몇 가지 테케에서 시간초과가 발생하였다. 이 함수가 범인이었다!

시간초과를 유발한 문제점 

- 스테이지 단계마다 실패율을 구할 때, 유저 별 스테이지 기록(stages)을 확인하게 됨
  - 복잡도가 스테이지 개수 N x stages이기 때문에 최악의 경우 500*200,000 (어마무시,,,)

``` swift
func getFailureRate(_ num: Int, _ stages: [Int]) -> Float {
    var numerator = 0.0 // 분자
    var denominator = 0.0 // 분모
    
    stages.forEach() {
        // 스테이지 도달했지만 클리어 하지 못한 경우
        if $0 == num { numerator += 1 }
        // 스테이지 도달 또는 클리어한 경우
        if $0 >= num { denominator += 1}
    }
    return numerator != 0 ? Float(numerator) / Float(denominator): 0
}
```

### 2️⃣ 변경한 코드

그래서 위 블로그를 참고하여 무조건 한번은 stages를 접근해야 하므로 이를 이용해서 적은 접근으로 실패율을 구하는 방법으로 수정하였다.

1. 스테이지별 도달한 사람 수를 저장하는 `reachCount` 배열에 저장
2. 실패율 구하기 
   1. 문제에서 언급했듯이 `실패율 = 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수`
   2. 아래 주석처럼 도달했으나 클리어하지 못하는 경우는 `reachCount`를 접근하여 n^n였던 1번째 방법을 n 복잡도로 줄여 실패율을 구할 수 있게 되었다.
   3. 분자가 0이 되면 안되므로 삼항연산자로 조건을 걸어줌
3. key값 기준 오름차순 정렬 후, 실패값인 value 기준 내림차순 정렬하여 key값들을 배열로 return

``` swift
import Foundation

func solution(_ N:Int, _ stages:[Int]) -> [Int] {
    var failureRates: [Int: Float] = [:]
    var reachCounts: [Int] = Array(repeating: 0, count: N + 1)
    var answer: [Int] = []
    
    // 1. 스테이지별 도달한 사람 수 구하기
    for stage in stages {
        for i in 0..<stage {
            reachCounts[i] += 1
        }
    }
    
    // 2. 실패율 구하기
    // -2 도달하였으나 클리어 하지 못한 경우 = 해당 스테이지 도달 수 - 다음 스테이지 도달 수
    for i in 0..<N {
        var notClear = reachCounts[i] - reachCounts[i+1]
        failureRates[i+1] = notClear != 0 ? Float(notClear) / Float(reachCounts[i]): 0
    }
    answer = failureRates.sorted(by: <).sorted { $0.value > $1.value }.map { $0.key }
    
    return answer
}
```



## 3️⃣ 이 문제를 풀면서 되새긴 점

### 딕셔너리는 순차를 가지는 CollectionType이 아니다!

for문으로 key값을 순차적으로 접근해 딕셔너리 값을 생성했어도, 실제 순서는 순차적으로 저장되지 않는다.

<image src="https://user-images.githubusercontent.com/47033052/144550992-5986b706-6b63-4930-bc0b-67379a58094e.png" width="30%" />

이번 문제처럼 같은 value를 가질 때, 작은 key를 앞으로 놓아야하는 조건이 있다면, 딕셔너리 자체를 오름차순 정렬해주고 필요한 정렬을 해야 한다.

``` swift
result = dictionary.sorted(by: <).sorted { /* 하고 싶은 정렬 */ }
```

### 주먹구구식 보단 복잡도를 생각하는 풀이를 생각해보자

일단 문제를 푸는 것도 중요하지만, 풀이가 이게 틀릴 일이 없는데 시간 초과가 발생한다면 복잡도를 꼭 계산해보자!
