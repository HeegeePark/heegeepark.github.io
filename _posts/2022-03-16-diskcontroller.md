---
layout: post
title: "[알고리즘] 프로그래머스 디스크 컨트롤러 swift 풀이"
date: 2022-03-16 14:34:10 +0700
excerpt: Heap 문제를 정렬로 풀기!
categories: [Algorithm]
tags: [Algorithm, 정렬]
---

## 1️⃣ 문제

> [문제 - 디스크 컨트롤러](https://programmers.co.kr/learn/courses/30a/lessons/42627)
>
> [풀이 참조 - Blog](https://jeonyeohun.tistory.com/235)

## 2️⃣ 풀이 방법 및 코드

스케줄링 알고리즘인 SJF(Shortest Job First)을 이용하려고,

처음에는 가볍게 작업 시간이 짧은 순으로 작업하도록 했더니 턱도 없다 ^,^

그래서 보완하여 만든 절차는 다음과 같다.

```
1. 작업 요청 시간 빠른 순으로 jobs 정렬
	- 요청 시간 동일 시, 작업 시간 빠른 순으로
2. 가장 먼저 도착한 작업 수행
	- 1) 큐에서 제거
	- 2) 현재 시간에 작업 소요 시간 더하여 현재 시간 재설정
	- 3) 처리 시간(현재 시간 - 작업 요청 시간) 더해주기
3. 작업 수행 동안 도착한 작업들은 큐에 삽입(작업 시간 짧은 순으로)
4. 모든 작업을 처리할 때 까지 2,3 과정 반복
	- 요청에 텀이 있는 경우(미수행 작업 있음에도 큐가 비었을 시) : 그 중 가장 빨리 도착하는 작업 큐 삽입 및 현재시간 작업의 요청시간으로 변경
5. 답 = 처리 시간 합 / 작업 개수
```

- 여기서의 KEY POINT는 수행 시간 중 도착한 작업들 끼리 (큐 안에서) 작업 시간이 짧은 순으로 재정렬해주어야 한다.

``` swift
import Foundation

func solution(_ jobs:[[Int]]) -> Int {
    var queue = [[Int]]()
    var timeline = 0
    var processingtime = 0
    
    // 1. 작업 요청 시간 빠른 순, 같을  시 작업 시간 짧은 순으로 정렬
    var sortedJobs = jobs.sorted {
        if $0.first == $1.first! { return $0.last! < $1.last!}
        else { return $0.first! < $1.first! }
    }
    
    // 2. 처음 할 작업만 작업 큐에 삽입
    queue.append(sortedJobs.removeFirst())
    timeline = queue.first![0]
    
    // 3. 작업 수행
    while !queue.isEmpty {
        let task = queue.removeFirst()
        timeline += task.last!
        processingtime += timeline - task.first!
        
        // 3-1. 작업 수행 동안 도착한 작업들만 작업 큐에 작업 시간 짧은 순으로 저장
        while !sortedJobs.isEmpty && sortedJobs.first![0] <= timeline {
            queue.append(sortedJobs.removeFirst())
        }
        queue.sort { $0.last! < $1.last! }
        
        // 3-2. 요청 텀 때문에 작업이 남았는데 큐가 비었을 경우
        if queue.isEmpty && !sortedJobs.isEmpty {
            // 가장 빨리 도착하는 작업 큐에 넣고 현재 시간 초기화
            queue.append(sortedJobs.removeFirst())
            timeline = queue.last![0]
        }
    }
    
    return processingtime / jobs.count
}

```



## 3️⃣ 이 문제를 풀면서 되새긴 점

이 문제는 원래 heap 우선순위 큐로 푸는 방법이 유명한데, swift에서는 라이브러리 지원이 안되어서 이렇게 정렬로 풀어도 충분히 해낼 수 있었다.

3학년 때 스케줄링 알고리즘 구현 과제 했던게 조금 도움이 된 듯!?
