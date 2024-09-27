---
title: "Tower of Hanoi, HJM Data Structures 4-3"
date: 2024-09-27 12:00:02 +0900
categories: [Data Structures, HJM Data Structures]
tags: [data_structures, cpp]
use_math: true

typora-root-url: ../

## NOTICE BLOCK
# {: .notice}
#<div class="notice">
#{here}  
#</div>

## FADED ITALIC
# {: .faded_italic-text}

## DIMMED-TEXT
# {: .dimmed-text}

### FADED_ITALIC + DIMMED-TEXT
# <span class="faded_italic-text dimmed-text">{here}</span>

## Comment... 이미지 설명글자 넣기
#<figure style="text-align: center;">{image}<figcaption>{dsc}</figcaption></figure>

---

<div class="notice">
이 글은 홍정모 교수님의 자료구조 강의에 기반한 글입니다.<br>
  🌟<a href="https://honglab.co.kr/courses/data-structures" target="_blank">들으러가기</a>
</div>

## Tower of Hanoi

3개의 기둥이 있고 하나의 기둥에 디스크의 크기가 가장 큰 것부터 아래로 깔려있는 디스크들을 같은 정렬 방식으로 다른 기둥으로 옮기는 게임.  
기둥에서 꺼낼 때는 가장 위에 있는 것만 꺼내 옮길 수 있다.  
문제는 이것을 재귀함수를 이용하여 구현하는것.

<figure style="text-align: center;"><img src="/../images/2024-09-27-hjmDataStructures-4-3/captureDecran22n.png" alt="captureDecran22n" style="zoom:60%;"/><figcaption><a href="https://www.mathsisfun.com/games/towerofhanoi.html" target="_blank">플레이 해보기</a></figcaption></figure>

이 문제를 처음 접해봤는데 처음 디스크 3개로 시작할 때 반대쪽 봉으로 옮기는 것은 어렵지 않다. 하지만 4개, 5개로 가면 확실히 난해해졌다.   
디스크가 많을 땐 도저히 직관으로는 어떻게 스탭들을 규칙적으로 밟아나가야 할 지 감을 못 잡았다.  
<span class="faded_italic-text dimmed-text">구조 파악 없이 코드를 짜낼 수 있을리는 만무하다</span>

> 그래도 하나 찾은 작은 실마리는 이사를 간다면 일단 가장 아래부터 차차 쌓아야 한다는 것.  
> 가장 아래 디스크를 먼저 옮겨둬야한다

---

## 추정하기 

### 최소 그룹을 먼저.

간단한 예로 디스크가 4개 있다고 하고 각각 아래부터 n-3, n-2, n-1, n이 쌓인다고 해보자.

<br>

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_431CD9A88B5A-1.jpeg" alt="IMG_431CD9A88B5A-1" style="zoom: 50%; text-align: center;" />  

> 가장 아래 디스크인 `n-3`을 3번째 기둥에 옮기려고 봤지만 나머지 위 디스크 `n-2`, `n-1`, `n` 때문에 옮기지 못한다.  
> 이들을 먼저 다른 데에 옮겨두자.  
>
> 편의상 `n-3`을 잠시 가려보겠다.

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_E7488A581DC6-1.jpeg" alt="IMG_E7488A581DC6-1" style="zoom:50%;" />

> 가장 아래 디스크인 `n-2`를 옮기려고 봤지만 나머지 위 디스크 `n-1`, `n` 때문에 옮기지 못한다.  
> 이들을 먼저 다른데에 옮겨두자.
>
> 편의상 `n-2`을 잠시 가려보겠다.

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_5BE16D7F8809-1.jpeg" alt="IMG_5BE16D7F8809-1" style="zoom:50%;" />

> 🌟여기선 `n-1`을 가리고 `n`을 따지는 것은 의미를 상실한다. <span class="faded_italic-text dimmed-text">어디다 두든지 상관없잖아..</span>      
>
> 따라서 가장 마지막 두 디스크의 이동이, 하노이의 탑 디스크들의 이동과 관련하여 `최소 논리 단위`가 된다.  
> 여기서 규칙을 찾고 연쇄시켜서 역으로 올라가면 전체의 이동규칙을 찾을 수 있게 된다.

---

### 어떻게 이동되는가?

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_C183301327CD-1.jpeg" alt="IMG_C183301327CD-1" style="zoom:40%; text-align: left;" />

> 잠시 약어 정리,
>
> 가장 아래있는 디스크를 `me`,  
> 나머지 위 디스크들을 `head`,  
> 시작점을 `f`,  
> 목표점을 `to`,  
> 남는 임시공간을 `tm`라 하자.

---

