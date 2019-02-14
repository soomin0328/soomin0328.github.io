---
layout: post
title: Sort - 프로그래머스
subtitle: 프로그래머스 Sort 문제들(3개)
date:   2019-01-07 12:00
categories: algorithm
tags: algorithm
sitemap :
  changefreq : daily
  priority : 1.0
---
출처: [프로그래머스](https://programmers.co.kr/learn/courses/30/parts/12198)  
언어: JAVA

# 1. K번째수
### 1) 문제 설명
[K번째수](https://programmers.co.kr/learn/courses/30/lessons/42748?language=java)

### 2) 풀이 과정
주어진 배열을 n번째 ~ m번째로 잘라 새로운 배열을 만든다음 정렬한 후, 새로운 배열에서 k번째 숫자를 찾으면되는 간단한 문제다.

### 3) 코드
```java
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];
        int newArr[] = null;

        for (int i = 0; i < commands.length; i++) {
            newArr = new int[(commands[i][1] - commands[i][0]) + 1];
            for (int j = 0; j < (commands[i][1] - commands[i][0]) + 1; j++) {
                newArr[j] = array[commands[i][0] - 1 + j];
            }
            Arrays.sort(newArr);
            answer[i] = newArr[commands[i][2] - 1];
            }

        return answer;
        }
}
```
# 2. 가장 큰 수
### 1) 문제 설명
[가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746?language=java)

### 2) 풀이 과정
주어진 배열에서 앞부터 차례대로 두 개의 숫자를 순서를 바꿔 이어보며 비교하면 된다.  
`a + b`와 `b + a`중에 어떤 숫자가 더 큰지 비교하여, 만약 `b + a`가 더 크다면 두 값의 순서를 바꿔준다.


예를들어 `[6, 10, 2]`라는 배열이 주어졌다고 하자.  
그럼 우선 첫 번째 값(a = 6)과 두 번째 값(b = 10)을 이용하여 `610`과 `106`을 만들 수 있는데, 여기서는 `610`이 더 크기 때문에 두 값의 순서를 바꾸지 않아도 된다.  
이렇게 배열의 모든 값들을 비교한 후, 배열의 순서대로 숫자들을 이어주면 제일 큰 수 `6210`이 나온다!!

### 3) 코드
```java
import java.util.*;

class Solution {
    public String solution(int[] numbers) {
        String answer ="";
        List<String> list = new ArrayList<>();
        int length = numbers.length;
 
        for(int i=0; i<length; i++){
            list.add(Integer.toString(numbers[i]));
        }

        int size = list.size();
        Collections.sort(list, (num1, num2) -> (num2+num1).compareTo(num1+num2));

        if(list.get(0).equals("0")){
            return "0";
        }

        for(int i=0; i<size; i++){
            answer = answer + list.get(i);
        }
        return answer;
	}
}
```
두 수를 비교하여 바꾸는 과정은 `Collections.sort()`와 `compareTo()`를 이용했다.

> **`compareTo()`**??  
> 두 개의 문자열을 비교하여 `int`형을 반환한다.  
> `a.compareTo(b)`
> - a == b : return 0
> - a > b : return 1;
> - a < b : return -1;

`compareTo()`한 결과가 0이나 1이면 오름차순으로 정렬되고, -1이면 내림차순으로 정렬된다.
<br>
# 3. H-Index
### 1) 문제 설명
[H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747?language=java)

### 2) 풀이 과정
이 문제는 H-Index에 대한 개념을 이해하는 것 자체가 너무 어려웠다&#128565;  
.  
.  
.  
to be continued,,,

### 3) 코드
```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        int answer = 0;

    Arrays.sort(citations);
    
    int hIndex = 0;
    int lastValue = citations[citations.length - 1];
    
    for (int i = 0; i <= lastValue; i++) {
        for (int j = 0; j < citations.length; j++) {
            if (i <= citations[j]) {
                if (i <= citations.length - j) {
                    answer = i;
                    break;
                }
            }
		}
	}

    return answer;
	}
}
```