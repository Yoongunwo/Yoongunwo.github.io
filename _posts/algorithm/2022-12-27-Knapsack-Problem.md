---
title: "Knapsack Problem"
categories:
  - 알고리즘
#tags:
#  - 알고리즘
toc: true
toc_label: "목차"
#toc_icon:
toc_sticky: true
#last_modified_at:
---

## 1. Kanpsack Problem이란?
> 도둑이 보석가게에 배낭을 메고 침입했다.
> 배낭의 최대 용량은 W이며, 이를 초과해서 보석을 담으면 배낭이 찢어질 것이다.
> 각 보석들의 무게와 가격은 알고 있다.
> 배낭이 찢어지지 않는 선에서 가격 합이 최대가 되도록 보석을 담는 방법은?

배낭 채우기 문제는 2가지로 나뉘는데 보석을 자를 수 있다고 가정하는 Fraction Knapsack 문제와 자를 수 없다고 가정하는 0-1 Knapsack 문제가 있다.\
0-1 Kanpsack 문제는 다이나믹 프로그래밍이라는 방법을 쓰는 기본적인 문제로 알려져 있다. 이 문제를 보면 생각나는 방법이 두 가지 정도가 있다.

#### 1.1 모든 경우의 수를 넣어본다 (Brute-Force)
n개의 보석이 있다고 치면, n개의 보석으로 만들 수 있는 가능한 부분집합의 수는 $2^n$개이다. 최악의 경우에는 $2^n$번까지 봐야 한다. 즉 $O(2^n)$의 시간복잡도가 나온다.

#### 1.2 가치가 제일 높은 보석(Greedy)

- 1.2.1 가격이 제일 높은 보석
> (4kg, `$`6), (4kg, `$`7), (2kg, `$`4), (1kg, `$`3), (6kg, `$`9), (3kg, `$`5)를 만족하는 보석들이 있고 가방의 한도는 10kg이라고 가정하자

가격이 제일 높은 보석을 먼저 고르는 방법을 쓴다고 하면, (4kg, `$`7), (6kg, `$`9)를 고를 것이다. 그러면 10kg이 되고 가격의 합은 `$`16이 된다.\
하지만 (4kg, `$`6), (4kg, `$`7), (2kg, `$`4)를 고르면 10kg/`$`17이 된다는 것을 알 수 있다. 즉 이 방법은 최적의 답을 보장하지 못한다.

- 1.2.2 (가격/무게)의 값이 제일 높은 보석
> W = 30kg\
> 보석1 : 5kg $\div$ `$`5 -> kg당 `$`1\
> 보석2 : 10kg $\div$ `$`6 -> kg당 `$`0.6\
> 보석3 : 20kg $\div$ `$`14 -> kg당 `$`0.7

이 경우에 **무게당 가격** 순으로 보석을 넣으면 보석1, 보석3이 들어가며 가격의 합은 `$`19이다. 그런데 배낭 무게가 5kg이 남는다. 보석2를 넣을 수는 없으므로 5kg은 남는 공간이 된다. 보석2, 보석3 을 넣으면 30kg/`$`20 가 된다.

이처럼 특정한 기준을 정해놓고 그 기준의 값이 가장 높은 순으로 집는 걸 Greedy 알고리즘이라고 하는데 0-1 배낭 채우기 문제는 Greedy한 방법으로는 풀 수 없는 문제이다.\
**Fraction Knapsack 문제는 Greedy로 풀 수 있다. 위 예시에서 보석2를 반으로 잘라서 남은 5kg짜리 공간에 넣으면 그게 최적의 답이 된다.**

## 2. DP 적용
0-1 배낭 채우기 문제를 푸는데 DP가 적용된다.
- 집합 A가 n번째 보석을 포함하고 있지 않다면 A는 n번째 보석을 뺀 나머지 n-1개의 보석들 중에서 최적으로 고른 부분집합과 같다.
- 집합 A가 n번째 보석을 포함하고 있다면, A에 속한 보석들의 총 가격은 n-1개의 보석들 중에서 최적으로 고른 가격의 합에다가 보석 n의 가격을 더한 것과 같다. (단 n번째 보석을 넣었을 때 배낭이 터지지 않아야 한다.)

이 내용을 점화식으로 작성하면 아래와 같다
> $P[i,w]$ = \
> if  $\(\; w_i > w,\) \quad P[i-1, w]$\
> else, $P[i,w] = max \[ v_i + P[i-1, w - w_k], P[i-1, w] \]$

P[i,w]란 i개의 보석이 있고 배낭의 무게 한도가 w일 때 최적의 이익을 의미한다. 식을 문장으로 풀면 아래와 같다.
- i번째 보석이 배낭의 무게한도보다 무거우면 넣을 수 없으므로 i번째 보석을 뺀 i-1개의 보석들을 가지고 구한 전 단계의 최적값ㅇ르 가져온다
- 그렇지 않은 경우, i번째 보석을 위해 i번째 보석만큼의 무게를 비웠을 때의 최적값에 i번째 보석의 가격을 더한 값 or i-1개의 보석들을 가지고 구한 전 단계의 최적값 중 큰 것을 선택한다.

P라는 2차원 리스트를 채워나가면서, 필요한 경우 "앞에서 계산했던 값을 이용해서" 다음 값을 계한다. 따라서 이게 DP문제인 것이다.

## Ref.
[Knapsack Problem](https://gsmesie692.tistory.com/113)\
