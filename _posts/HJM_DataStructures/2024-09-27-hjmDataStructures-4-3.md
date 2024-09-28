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
이 글은 홍정모 교수님의 자료구조 강의 4-3강에 기반한 글입니다.<br>
  🌟<a href="https://honglab.co.kr/courses/data-structures" target="_blank">들으러가기</a>
</div>


## Tower of Hanoi

3개의 기둥이 있고 하나의 기둥에 디스크의 크기가 가장 큰 것부터 아래로 깔려있는 디스크들을 같은 정렬 방식으로 다른 기둥으로 옮기는 게임.  
기둥에서 꺼낼 때는 가장 위에 있는 것만 꺼내 옮길 수 있다.  
문제는 이것을 재귀함수를 이용하여 구현하는것.

<figure style="text-align: center;"><img src="/../images/2024-09-27-hjmDataStructures-4-3/captureDecran22n.png" alt="captureDecran22n" style="zoom:60%;"/><figcaption><a href="https://www.mathsisfun.com/games/towerofhanoi.html" target="_blank">플레이 해보기</a></figcaption></figure>

이 문제를 처음 접해봤는데 처음 디스크 3개로 시작할 때 반대쪽 봉으로 옮기는 것은 어렵지 않다. 하지만 4개, 5개로 가면 확실히 난해해졌다.   
디스크가 많을 땐 도저히 직관으로는 어떻게 스탭들을 규칙적으로 밟아나가야 할지 감을 못 잡았다.  
<span class="faded_italic-text dimmed-text">구조 파악 없이 코드를 짜낼 수 있을리는 만무...</span>

> 하나 찾은 작은 실마리는 이사를 간다면 일단 가장 아래부터 차차 쌓아야 한다는 것.  
> 가장 아래 디스크를 먼저 옮겨둬야한다

---

## 추정하기 

### 최소 그룹을 먼저.

간단한 예로 디스크가 4개 있다고 하고 각각 아래부터 n-3, n-2, n-1, n이 쌓인다고 가정해봤다.

<br>

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_431CD9A88B5A-1.jpeg" alt="IMG_431CD9A88B5A-1" style="zoom: 50%; text-align: center;" />  

> 가장 아래 디스크인 `n-3`을 3번째 기둥에 옮기려고 봤지만 나머지 위 디스크 `n-2`, `n-1`, `n` 때문에 옮기지 못한다.  
> 이들을 먼저 임시공간에 옮겨두자.  
>
> 편의상 `n-3`을 잠시 가려보겠다.

<img src="/../images/2024-09-27-hjmDataStructures-4-3/IMG_E7488A581DC6-1.jpeg" alt="IMG_E7488A581DC6-1" style="zoom:50%;" />

> 가장 아래 디스크인 `n-2`를 옮기려고 봤지만 나머지 위 디스크 `n-1`, `n` 때문에 옮기지 못한다.  
> 이들을 먼저 임시공간에 옮겨두자.
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

<br>

![IMG_9D631A2D9F9A-1](/../images/2024-09-27-hjmDataStructures-4-3/IMG_9D631A2D9F9A-1.jpeg)

먼저  `최소 단위`에서 이동할 때는 어떻게 해야하는지 직접 그려보면 위 그림과 같이 `시작점 f`에서 `목표점 to`로 옮길 수 있다.  

> 이동 과정 :
>
> 1. `head`가 `tm`로 간다. = `head` → `tm`
> 2. `me`가  `to`로 간다. = `me` → `to`
> 3. `tm`에 있던 것이 `to`로 간다. = `tm` → `to`

---

### 이동 관계식

결국 이를 정리하면 시작점에서 도착지로 가는 `<f→to>`는 다음과 같이 나타낼 수 있다.  
"+"는 순서진행을 나타낸다. `첫번째항` 시행 → `두번째항` 시행 → `세번째항` 시행.

![IMG_1CC07D96CBC0-1](/../images/2024-09-27-hjmDataStructures-4-3/IMG_1CC07D96CBC0-1.jpeg)

### 논리점화 & 재귀

이 관계식은 디스크가 2개일 때만 아니라 3, 4개일때도 마찬가지로 식을 사용할 수 있다. 애당초 `head`는 아래 디스크 제외 나머지 위 디스크를 의미하기도 했다. 이는 한 개 또는 여러개가 될 수 있다.  
결국 디스크가 3개일때도, 4개일때도 이 관계식이 적용이 되는지 체크를 하여 귀납추론을 하는것이다.

이 관계식이 디스크의 이동과 관련한 식이라는 것을 집중해보자.  
그렇다면 우변의 첫째 항 `(head→tm)`을 보면 이 또한 디스크의 이동이다. → 관계식 또 적용 가능

이런식으로 하나의 항에서도 또 관계식으로 전개가 되어 가지가 뻗어나가는 것이 나중에 사용할 재귀의 개념이다.  
아래는 위 갈색 이동관계식에서 예시로 첫째항을 딱 한 번 재귀시켜봤다. 물론 `f`의 디스크 개수가 3개 이상일때.

<br>

![IMG_B16F99842ACD-1](/../images/2024-09-27-hjmDataStructures-4-3/IMG_B16F99842ACD-1.jpeg)

> - 주황색은 인덱스 번호이다
> - `(i)`에서 첫항을 재귀시키면 `(ii)`로 전개된다.
>   - 이때 `(i)` 첫째항에서의 `head`, `tm`는 각각  `(ii)`에서 `f`와 `to`가 된다.  
>     \>> `head`가 출발지 역할, `tm`이 도착지 역할이니.
>   - 따라서 재귀 후인 `(ii)`에서 tm, to는 원래 태초의 tm, to와들과는 다른 자리를 가리키게 된다. 이점을 주의하자.    
>     인덱스 번호 참고.
>   - 결국 `(ii)`에서 `f`가 인덱스 0자리 이고 `to`가 인덱스 1자리이면 남는 인덱스 2를 `tm`이 담당하게 된다.
>   - 이렇게 각 f, tm, to가 달라지는 것은 재귀가 실행될때마다 스택이 하나 쌓이는 것과 유사하다.

---

## disk가 3개일때 귀납 추론 검증

![IMG_0A4B21869E9B-1](/../images/2024-09-27-hjmDataStructures-4-3/IMG_0A4B21869E9B-1.jpeg)

모든 전개는 철저히 이동 관계식을 따른다.

- `(i)`의 첫째항에서 `f`자리에 디스크가 2개(`n-1`, `n`)가 있으므로 `(ii)`로 재귀한다.
  - `(ii)`에서 첫~셋째항은 모두 `f`자리에 디스크가 1개이므로 재귀할필요없이 시행이 모두 끝나면 `(i)`의 첫째항으로 복귀한다.
- `(i)`의 둘째항은 `f`자리에 디스크가 하나이므로 재귀할필요없이 이동 시행해준다.
- `(i)`의 셋째항은 `f`자리에 디스크가 2개(`n-1`, `n`)가 있으므로 `(iii)`로 재귀한다.
  - `(iii)`는 모두 디스크 하나들의 이동이므로 시행이 모두 끝나면 `(i)`로 복귀하고 결국 전체 디스크 이동은 전부 끝난다.

물론 귀납추론이라는 이름이 무색하게 [가장 처음 예시를 들었던 것](#최소-그룹을-먼저)만으로도 위 이동관계식이 맞음을 알 수 있다.

---

## 코드 구현

### 소스코드

이제 이를 코드로 구현하자.

- `Stack`의 배열을 이용, `Stack[0]`, `Stack[1]`, `Stack[2]`들이 세 기둥이다.
- `void PrintTowers()`는 각 기둥 별 요소들을 보기좋게 출력해준다.
- `void MoveDisk(int from, int to)`는 한 기둥에서 가장 위 디스크를 집어 다른 기둥으로 옮기는 기능을한다.
  - 다른 기둥으로 갖다 놓을 때는 `A, B, C ...`<span class="faded_italic-text dimmed-text">알파벳 오름차순</span> 순서 처럼 놓게 된다.
  - 이동이 끝나면 전체 출력한다. `void PrintTowers()`

```cpp
#include <iostream>
#include <share/stack.h> //이전 강의에서 구현한 stack, 없으면 std::stack이용

using namespace std;

Stack<char> tower[3];

void PrintTowers()
{
	cout << "Towers" << endl;
	cout << "0: ";
	tower[0].Print();
	cout << "1: ";
	tower[1].Print();
	cout << "2: ";
	tower[2].Print();
}

//실제로 디스크를 움직여서 스택들을 업데이트 (다른 교재들에는 X)
void MoveDisk(int from, int to)
{
	if (tower[from].IsEmpty())
	{
		cout << "Tower " << from << "is empty." << endl;
		exit(0); // 오류 강제 종료
	}

	auto disk = tower[from].Top();

	//받을 타워가 비어 있으면 뭐든지 받을 수 있음
	//알파벳 순서여야 받을 수 있음 (역순 X)
	if (!tower[to].IsEmpty() && tower[to].Top() > disk)
	{
		cout << "Cannot place " << disk << " on " << tower[to].Top() << endl;
		exit(0); // 오류 강제 종료
	}

	tower[from].Pop();
	tower[to].Push(disk);

	cout << "Move disk " << disk << " from " << from << " to " << to << endl;
	PrintTowers();
}
```

- `num_disks`는 초기 디스크 개수
- `RecurMoveDisks()` 의 파라미터는 `RecurMoveDisks(디스크 개수, from의 인덱스, temp의 인덱스, to의 인덱스);`
  - 아까 언급된 `f`가 from, `tm` = temp, `to` = to.

```cpp
int main()
{
	int num_disks = 3;

	for (int i = 0; i < num_disks; i++)
		tower[0].Push('A' + i);

	PrintTowers();

	RecurMoveDisks(num_disks, 0, 1, 2);

	PrintTowers();

	return 0;
}
```

---

### 구현하기

```cpp
void RecurMoveDisks(int n, int from, int temp, int to)
{
	...
}
```

![IMG_0A4B21869E9B-1](/../images/2024-09-27-hjmDataStructures-4-3/IMG_0A4B21869E9B-1-7469068.jpeg)

이 도식을 다시 참고해보자.

가지가 뻗어져나가는 저 화살표는 재귀를 의미한다. 또한 재귀를 하면서 언제 재귀를 멈출지도 정해야한다.  
그렇다면 일단 첫째항, 둘째항, 셋째항 모두 각각 재귀함수를 여는 코드를 적어봤다.

```cpp
RecurMoveDisks( , , , ); // 1st term
MoveDisk( , ); // 2nd term
RecurMoveDisks( , , , ); // 3rd term
```

- 재귀 종료 조건
  - (i)에서는 `<f→to>`에서 f에 디스크가 3개있다.  and   (ii)에서는 2개가 된다.  
    (ii)의 첫째항에서 또 재귀가 열릴텐데 새로 재귀가 열린다면 그쪽에서`<f→to>`의 f는 디스크가 1개, 즉 `n=1`일것이다.
  - 저때 멈추면 되므로 일단 마지막 상태에서 재귀를 시켜주고 변화를 주기전에 `n==1` 검사를 먼저 하고 종료되어야 하는 상태이면
    디스크 이동을 시키고 복귀한다.
  - 처음 함수가 시작될때는 `n=3`이다. 따라서 재귀될때마다 `n`을 `-1`하면 된다.

- 첫째항:

  `(i)`에서 첫째항이 재귀식`(ii)`을 열때
  자신의 `head(f)`를 (ii)의 `f` 파라미터로 넘기고
  자신의 `tm`를 (ii)의 `to` 파라미터로 넘긴다.

  따라서 `RecurMoveDisks(n-1 , from,  , temp)`이다... 남는 자리는 to가 가져간다.
  ==> `RecurMoveDisks(n-1 , from, to, temp)`

- 둘째항:

  디스크 하나의 이동 관련이니 `MoveDisk(from, to)`를 쓴다.

- 셋째항:

  `(i)`에서 셋째항이 재귀식 `(iii)`을 열 때
  자신의 `tm`를 (iii)의 `f` 파라미터로 넘기고
  자신의`to`를 (iii)의 `to` 파라미터로 넘긴다.

  따라서 `RecurMoveDisks(n-1 , temp,  , to)` 이다.... 남는 자리는 from이 가져간다.
  ==>`RecurMoveDisks(n-1 , temp, from, to)`

==>

```cpp
	RecurMoveDisks(n - 1, from, to,temp); // 1st term
	MoveDisk(from, to); // 2nd term
	RecurMoveDisks(n - 1, temp, from, to); // 3rd term
```

<br>

앞서 말한 재귀 종료 조건문도 넣어야 한다. 위에서 설명한 대로 추가 액션 전에 검사를 해야 되니 위 코드보다는 상단에 배치한다.

중간결과 ::

```cpp
void RecurMoveDisks(int n, int from, int temp, int to)
{
    if (n == 1)
    {
			//...
       return;
    }

    RecurMoveDisks(n - 1, from, to,temp); // 1st term
    MoveDisk(from, to); // 2nd term
    RecurMoveDisks(n - 1, temp, from, to); // 3rd term

}
```

끝날때가 되면 재귀 종료를 하러 새로 재귀함수를 열고 `if`조건문 안에 들어간다.

예컨대 `//1st term`에서 왔다면 일단 디스크의 개수가 하나인 상태에서 왔다. 이때 중요한건 이 디스크를 어디서 (`from`), 어디로(`to`) 옮길것인지다.

결국 새로 열리는 재귀종료를 위한 재귀함수의 `from`자리에 자신의 시작점인 `from`을, `to`자리에 자신의 목표점인 `temp`를 넣어주게 된다.  
그래서 가장 마지막에 `MoveDisk(from, to);`가 실행되면 원하는 대로 잘 옮겨질 것이다.

그 후 `return`을 만나 이전 스택으로 복귀한다.

### 최종코드

```cpp
void RecurMoveDisks(int n, int from, int temp, int to)
{
    if (n == 1)
    {
       MoveDisk(from, to);
       return;
    }

    RecurMoveDisks(n - 1, from, to,temp); // 1st term
    MoveDisk(from, to); // 2nd term
    RecurMoveDisks(n - 1, temp, from, to); // 3rd term

}
```

원하는 최소이동횟수로 디스크들이 잘 이동된다.



```cpp
void RecurMoveDisks(int n, int from, int temp, int to)
{
	if (n==0)
		return;

	RecurMoveDisks(n - 1, from, to,temp); // 1st term
	MoveDisk(from, to); // 2nd term
	RecurMoveDisks(n - 1, temp, from, to); // 3rd term
}
```

위는 교수님의 코드.

역시나 위 도식을 보며 따라가면 코드 이해는 어렵지 않다.  
아까 위 도식의 `(ii)`이 마지막 층인 지하 2층이었다면, 여기서는 지하 3, 4층까지 갔다가 4층에서 돌아오고 3층에서 `MoveDisk()`를 만나 원하는 이동을 하고 원래 `(ii)`로 돌아오는 형식이다.

내건 지하3층에서 바로 종료검사하고 이동시켜주고 복귀하는 형태.

> 결국 간단해 보일 수도 있는 문제이지만 주저리주저리, 오지랖 넓은 긴 설명..  
> 그래도 복기 하면서 다시 잘 곱씹어볼 수 있게 됐다.