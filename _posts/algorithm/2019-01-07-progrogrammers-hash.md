---
layout: post
title: Hash - 프로그래머스
subtitle: 프로그래머스 Hash 문제들(4개)
date:   2019-01-07 12:00
categories: algorithm
tags: algorithm
sitemap :
  changefreq : daily
  priority : 1.0
---
출처: [프로그래머스](https://programmers.co.kr/learn/courses/30/parts/12077)  
언어: JAVA

# 1. 완주하지 못한 선수
### 1) 문제 설명
[완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576?language=java)

### 2) 풀이 과정
이 문제는 참여자 명단과 완주자 명단을 정렬하여 두 명단의 같은 `index`의 값을 비교하여 해결 할 수 있다. 만약 참여자 명단에 있는 모든 사람이 완주자 명단에도 있다면&#10068; 배열을 정렬했을 때, 두 배열은 완벽히 같을 것이다.(같은 index의 값이 모두 같다!) 따라서 같은 `index`의 값만 비교해준다면 누가 완주하지 못했는지 알 수 있다!

- 참여자 명단

| 0      | 1      | 2      |
|--------|--------|--------|
| "eden" | "kiki" |  "leo" |

- 완주자 명단

| 0      | 1      |
|--------|--------|
| "eden" | "kiki" |

<br>
위의 두 표는 첫 번째 입출력 예의 참여자 명단과 완주자 명단을 정렬한 결과다(index와 값). 얼핏 봐도 `index 0`과 `index 1`의 값은 두 배열이 같은데 `index 2`의 값은 참여자 명단에만 존재한다! 따라서 "leo"는 참여만했지 완주하지 못한 것이다!!

### 3) 코드

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        Arrays.sort(participant);
        Arrays.sort(completion);
        int i;
        for(i=0; i<completion.length; i++){
            if(!participant[i].equals(completion[i])){
                return participant[i];
            }
        }
        return participant[i];
    }
}
```

`Arrays.sort()`로 두 배열을 정렬한 후, `for문`을 이용하여 두 배열의 index의 값을 비교하여 두 값이 다를 때 그 값을 `return`했다.

# 2. 전화번호 목록
### 1) 문제 설명
[전화번호 목록](https://programmers.co.kr/learn/courses/30/lessons/42577?language=java)

### 2) 풀이 과정
phone_book 배열에서 첫 번째부터 순서대로 pre로 정하여 배열 안의 pre를 제외한 다른 값들과 각각 비교한다. 두 값을 비교할 때, pre가 다른 값으로 시작하는지 알아보기 위하여 `startsWith()`라는 메소드를 이용했다. `startsWith()`를 몰랐다면 코드의 줄이 더 많았을 것이다...

### 3) 코드

```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        String pre = "", post = "";
        
        for(int i=0; i<phone_book.length; i++){
            pre = phone_book[i];
            for(int j=0; j<phone_book.length; j++){
                if(i!=j){
                    post = phone_book[j];
                    if(pre.startsWith(post)){
                        answer = false;
                        break;
                    }
                }
            }
        }
        
        return answer;
    }
}
```
<br>
# 3. 위장
### 1) 문제 설명
[위장](https://programmers.co.kr/learn/courses/30/lessons/42578?language=java)

### 2) 풀이 과정
이 문제는 스파이가 갖고있는 의상의 종류마다 갯수를 구해서 경우의 수를 구하면 된다.  
종류의 갯수는 `HashMap`을 이용하여 카운트 할 수 있다.  

### 3) 코드
```java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
		HashMap<String, Integer> spy = new HashMap<String, Integer>();
		int answer = 1;

		for (int i = 0; i < clothes.length; i++) {
			if (spy.containsKey(clothes[i][1])) {
				spy.put(clothes[i][1], spy.get(clothes[i][1]) + 1);
			} else {
				spy.put(clothes[i][1], 1);
			}
		}

		for (int v : spy.values()) {
			answer *= v+1;
		}

		return answer - 1;
	}
}
```

첫 번째 `for문`에서는 `HashMap`에 값을 넣는 과정인데, `containsKey()`를 이용하여 만약 `HashpMap`에 이미 해당 key가 포함되어 있다면, 새로운 값을 `HashMap`에 추가하는 것이 아니라 해당 key의 value 값을 **+1** 해준다. 그럼 결국

| Key      | Value   |
|----------|---------|
| headgear | 2       |
| eyewear  | 1       |

위의 표 처럼 `HashMap`에 들어갈 것이다.  
이제 이 값으로 경우의 수를 구해야 하는데, 경우의 수 구하는 법을 몰라 검색해 봤더니 모든 값에 +1을 해서 다 곱한 후 -1을 해주면된다더라,,ㅎ  
<br>
# 4. 베스트 앨범
### 1) 문제 설명
[베스트 앨범](https://programmers.co.kr/learn/courses/30/lessons/42579?language=java)

### 2) 풀이 과정
우선 노래들을 장르별로 모아서 장르별 총 재생 횟수를 구해준다. 그 다음 내림차순으로 정렬하여 총 재생 횟수가 가장 많은 장르부터 앨범에 넣어야 한다. 여기까지는 순조로웠지만,, 장르마다 또 따로 재생횟수로 정렬을 해주는 부분에서 많은 시간이 들었다.  
결국 생각해낸 방법은, 각각의 `index`값을 알면 어떻게 정렬하든 원하는 노래를 앨범에 넣을 수 있지 않을까? 라는 방법이었다.


![입출력 예](/img/ex1.PNG)
위의 예시로 설명을 하자면

![ex2](/img/ex2.png)  
- `gMap`&#128073; Key: index, Value: 장르 를 갖는 `HashMap`
- `pMap`&#128073; Key: index, Value: 재생 횟수 를 갖는 `HashMap`
- `gen`&#128073; Key: 장르, Value: 장르별 총 재생 횟수 를 갖는 `HashMap`

`gen`에서 첫 번째부터 순서대로 해당 장르가 `gMap`의 어느 `index`에 속하는지 확인한다. 해당 장르가 존재하는 index는 `indexArr`라는 `ArrayList`에 넣어준다.(이 때, `gen`에서 다음 장르로 넘어갈 때 `indexArr`를 초기화 해줘야한다.)


![ex3](/img/ex3.png)  
그럼 위의 사진처럼 `indexArr`가 첫 번째는 'pop'장르에 대해, 두 번째는 'classic'장르에 대해 만들어지게 될 것이다.  
이제 이 `index`를 갖고 `pMap`에서 각각 장르마다 재생 횟수를 비교해 제일 TOP2 노래를 골라서 앨범에 넣으면 끝이다~!

### 3) 코드
```java
for (int i = 0; i < genres.length; i++) {
	gMap.put(i, genres[i]);
	pMap.put(i, plays[i]);

	if (gen.containsKey(genres[i])) {
		gen.put(genres[i], gen.get(genres[i]) + plays[i]);
	} else {
		gen.put(genres[i], plays[i]);
	}
}
```
위 코드는 각각의 `HashMap`에 값들을 넣어준다. 이 때 `gen`에는 `containsKey()`를 이용해 총 재생 횟수를 넣어준다.

```java
Iterator it = sortByValue(gen).iterator();

public static List sortByValue(final Map map) {

		List<String> list = new ArrayList();

		list.addAll(map.keySet());

		Collections.sort(list, new Comparator<Object>() {

			public int compare(Object o1, Object o2) {
				Object v1 = map.get(o1);
				Object v2 = map.get(o2);
				return ((Comparable) v2).compareTo(v1);
			}

		});

		return list;

	}
```
`gen`을 내림차순으로 정렬해주는 코드이다.
```java
while (it.hasNext()) {
	String temp = (String) it.next();
	int index = 0;

	for (String gMapValue : gMap.values()) {
		if (gMapValue.equals(temp)) {
			indexArr.add(index);
		}
		index++;
	}

	getBest(indexArr);

	indexArr.clear();
}
```
`index`값을 `indexArr`에 넣어주는 과정이다. `getBest()`메소드는 `indexArr`값을 이용해 `pMap`에서 각 장르별로 재생 횟수가 많은 TOP2 노래를 골라주는 메소드이다.  


전체 코드는 ["여기"](https://github.com/Team-AiK/TT-Thinking-Training/blob/master/Week16/soom/Solution4.java)를 보면 된다.  
<br>