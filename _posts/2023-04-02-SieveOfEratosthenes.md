---
title: "에라토스테네스의 체"
categories:
  - 알고리즘
#tags:
#  - 알고리즘
#  - Greedy
toc: true
toc_label: "목차"
#toc_icon:
toc_sticky: true
#last_modified_at:
---

## 1. Sieve of Erathosthenes, 에라토스테네스의 체
고대 그리스의 수학자 에라토스테네스가 만들어 낸 소수를 찾는 방법이다. 이 방법은 마치 체로 치듯이 수를 걸러낸다고 하여 '에라토스테네스의 체'라고 부른다. 

## 2. 알고리즘
임의의 자연수 n에 대해 그 이하의 소수를 찾느 가장 간단하고 빠른 방법이다.\
예를 들어 1~100까지 숫자 중 소수를 찾는다고 하자.
> 일단 1부터 100까지 숫자를 쓴다.

![에라토스테네스의 체1](https://user-images.githubusercontent.com/97718735/229333881-9bb5200f-1fb8-4143-936f-2636bbffb561.png)

> 소수도, 합성수도 아닌 유일한 자연수 1을 제거한다.

![에라토스테네스의 체2](https://user-images.githubusercontent.com/97718735/229333882-add6ed4f-ec19-4d3a-aa3f-8402ad05ad07.png)

> 2를 제외한 2의 배수를 제거한다.

![에라토스테네스의 체3](https://user-images.githubusercontent.com/97718735/229333885-2b794a73-6606-4020-8af1-d4ca903d3476.png)

> 3을 제외한 3의 배수를 제거한다.

![에라토스테네스의 체4](https://user-images.githubusercontent.com/97718735/229333879-9fce23d4-4925-4348-90ac-d246f2a218c5.png)

> 4의 배수는 지울 필요 없다 (2의 배수에서 이미 지워졌기 때문).\
그러면 2,3 다음으로 남아있는 가장 작은 소수 5를 제외한 5의 배수를 제거해야 한다.

![에라토스테네스의 체5](https://user-images.githubusercontent.com/97718735/229333887-d715581c-360f-4119-8d00-4752047731f6.png)

> 그리고 마지막으로 7을 제외한 7의 배수까지 제거하면 결과는 아래와 같다.

![에라토스테네스의 체6](https://user-images.githubusercontent.com/97718735/229333877-f98efb09-a8fd-4eb7-a15a-6fdf1c5f8243.png)

이런식으로 남은 것들의 2배수, 3배수 ... n배수를 지우다 보면 소수만 남는다.

이런 방법으로 만약 n이하의 소수를 모두 찾고 싶다면 1부터 n까지 쭉 나열한 다음에 2의 배수, 3의 배수, 5의 배수 ... n의 배수까지 지운다.

일종의 노가다 방식이라 상당히 무식한 방법이긴 하지만, 특정 범위가 주어지고 그 범위 내의 모든 소수를 찾아야 하는 경우 에라토스테네스의 체보다 빠른 방법은 없다.

> 에라토스테네스의 체를 이용해 1~n까지의 소수를 알고 싶다면, n까지 모든 수의 배수를 다 나눠 볼 필요는 없다. 만약 n보다 작은 어떤 수 m이 $m = ab$ 라면 a와 b 중 적어도 하나는 $\sqrt{n}$ 이하이다. 즉 n보다 작은 합성수 m은 $\sqrt{n}$보다 작은 수의 배수만 체크해도 전부 지워진다는 의미이므로, $\sqrt{n}$ 이하의 수의 배수만 지우면 된다.

## 3. 코드
다음은 n이 1,000,000일 때의 예시이다.
- java 구현
```java
final static int SIZE = 1000000000;

public class Eratos{
    public static void main(String[] args){
        boolean[] eratos = new boolean[SIZE + 1];

        for(int i=2; i<=SIZE; i++){
            eratos[i] = true;
        }

        for(int i=2; i<=Math.sqrt(SIZE); i++){
            if(ertos[i] == false) continue;
            for(int j=i*2; j<=SIZE; j += i){
                eratos[j] = false;
            }
        }
    }
}
```

## Ref.
- [토르비욘-최대공약수(GCD)와 최소공배수(LCM)](https://torbjorn.tistory.com/244)

