---
layout: post
title: DP를 이용한 최적 전략 알고리즘
date: 2022-12-14
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: chess-g89190ca96_1920.jpg # Add image post (optional)
fig-caption: 체스 # Add figcaption (optional)
tags: [algorithm, optimal_strategy, dynamic_programming]
---

# 알고리즘: 최적 전략

<aside>
✏️ 배열 [2,7,40,19]의 양 끝에서 A와 B는 하나씩 숫자를 꺼낼 수 있다. 꺼낸 숫자의 합을 비교해서 합이 더 큰 사람이 게임에서 이긴다고 하고 A와 B는 모두 이길 수 있는 최적의 방법을 선택한다고 하자. 만약 A가 먼저 숫자를 꺼내기 시작했고, A가 이겼다고 했을 때 A와 B의 점수(숫자의 합)은 각각 몇 점일까?

</aside>


## A. 풀이 방법

### (1) 만약 배열에 숫자가 하나씩 들어있다고 하면?

→ A가 그 숫자를 뽑기 때문에 B의 점수는 0이 된다.

<br>

### (2) 만약 배열에 숫자가 2개씩 들어있다고 하면?

→ A가 두 수 중 큰 수를 뽑고 나머지 수를 B가 뽑는다.

<br>

### (3) 만약에 배열에 숫자가 3개씩 있다고 하면?

- [2,7,40]인 경우

| 2 | 7 | 40  |
|---|---|-----|


a) 만약 A가 2를 뽑는다고 하면 B는 7과 40 중에 큰 수인 40을 뽑을 것이고 A는 남은 7을 뽑는다.

→ A는 9점, B는 40점

b) 만약 A가 40을 뽑는다고 하면 B는 2와 7 중에 큰 수인 7을 뽑을 것이고 A는 남은 2를 뽑게 된다.

→ A는 42점, B는 9점

따라서 A는 최대 점수를 가질 수 있는 전략을 선택할 것이므로 b 전략을 선택하여 A의 점수는 42, B의 점수는 7이 된다.

<br>

- [7,40,19]인 경우

| 7 | 40 | 19 |
|---|----|----|

a) 만약 A가 7을 뽑는다고 하면 B는 40과 19 중에 큰 수인 40을 뽑을 것이고 A는 남은 19를 뽑는다.

→ A는 26점, B는 40점

b) 만약 A가 19를 뽑는다고 하면 B는 7와 40 중에 큰 수인 40을 뽑을 것이고 A는 남은 7를 뽑게 된다.

→ A는 26점, B는 40점

따라서 A는 최대 점수를 가질 수 있는 전략을 선택할 것이므로 a 또는 b 전략을 선택하여 A의 점수는 26, B의 점수는 40이 된다.

위의 상황을 다시 살펴보면 **A가 3가지 수 중에서 하나를 뽑으면 B는** 두 가지 수 중에서 큰 수를 뽑는다. 즉, **배열에 숫자가 2개가 있는 경우 A가 했던 것과 같은 전략을 사용함**을 확인할 수 있다.

<br>
<br>

### (4) 만약 배열에 숫자가 4개가 있다고 한다면?

| 2 | 7 | 40 | 19 |
|---|---|----|----|

a) A가 2를 먼저 뽑는다고 하면 B는 `배열 [7,40,19]`가 있는 경우에 최댓값을 뽑을 수 있는 전략을 펼칠 것이다. (3)의 두 번째 케이스를 보면 7을 뽑거나 19를 뽑더라도 모두 26이라는 동일한 점수를 얻기 때문에 B는 7을 뽑고 A는 40, B는 19를 뽑게 된다.

→ A는 42점, B는 26점

b) A가 19를 먼저 뽑았다고 하면 B는 `배열 [2,7,40]`이 있는 경우에 최댓값을 뽑는 전략을 펼칠 것이다. (3)의 첫 번째 케이스를 보면 2를 뽑는 경우는 9점을 가질 수 있지만, 40을 뽑으면 42점을 가질 수 있으므로 B는 40을 뽑을 것이고 A는 7, B는 2를 각각 순서대로 뽑을 것이다.

→ A는 26점, B는 42점

이 때 A는 최대 점수를 가질 수 있는 전략을 선택할 것이므로 a 전략을 선택하여 A의 점수는 42, B의 점수는 26점이 된다.

<br>

> ➡️ 따라서 숫자가 4개인 배열에서 숫자를 뽑는 경우는 3개인 배열에서 숫자를 뽑는 경우와, 숫자가 3개인 배열에서 숫자를 뽑는 경우는 2개인 배열에서 숫자를 뽑는 경우와 관련이 있으므로 Dynamic Programming을 이용해서 문제를 해결할 수 있다.




## B. 코드

```java
import java.util.Arrays;

class Pair<L, R>{
    L left;
    R right;

    Pair(L left, R right){
        this.left = left;
        this.right = right;
    }

}

public class OptimalStrategy {

    Pair[][] optimalSelect(int[] input){
        Pair[][] arr = new Pair[input.length][input.length];

        for (int i = 0; i < input.length; i++) {
            arr[i][i] = new Pair(input[i], 0);
        }


        for (int i = 0; i < input.length-1; i++) {
            arr[i][i+1] = new Pair(Math.max(input[i], input[i+1]), Math.min(input[i], input[i+1]));
        }

        for (int i = 2; i < input.length; i++) {
            for (int j = 0; j < input.length-i; j++) {
                int num1 = input[j] + (int) arr[j+1][j+i].right;
                int num2 = input[j+i] + (int) arr[j][j+i-1].right;

                if(num1 >= num2){
                    arr[j][j+i] = new Pair(num1, (int) arr[j+1][j+i].left);
                } else{
                    arr[j][j+i] = new Pair(num2, (int) arr[j][j+i-1].left);
                }



            }
        }



        return arr;

    }


    public static void main(String[] args) {

        OptimalStrategy optimalStrategy = new OptimalStrategy();

        Pair[][] result1 = optimalStrategy3.optimalSelect(new int[]{2, 7, 40, 19});

        for (int i = 0; i < result1.length; i++) {
            for (int j = i; j < result1.length ; j++) {
                System.out.printf("%d %d left: %d, right: %d\n", i, j, result1[i][j].left, result1[i][j].right);
            }

        }

    }
}

```


