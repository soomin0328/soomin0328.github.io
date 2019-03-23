---
layout: post
title: Stack/Queue - 프로그래머스
subtitle: 프로그래머스 Stack/Queue 문제들(6개)
date:   2019-02-13 12:00
categories: algorithm
tags: algorithm
sitemap :
  changefreq : daily
  priority : 1.0
---
출처: [프로그래머스](https://programmers.co.kr/learn/courses/30/parts/12081)  
언어: JAVA

# 1. 쇠막대기
[문제설명](https://programmers.co.kr/learn/courses/30/lessons/42585?language=java)

### 1) 풀이 과정
우선, `()` 이렇게 열고 닫히는게 연달아 있는 모양이 레이저가 있는 부분이고 다른 괄호들은 막대의 양 끝이라는 것을 알고 시작하자!  
전체 잘린 쇠막대기의 개수는 레이저들을 기준으로 이 전의 잘린 쇠막대기의 개수를 구해 다 더해주면 된다. &#128680;  그러나 주의해야 할 것은 쇠막대기는 레이저로 잘려서 총 개수가 늘어나지만 레이저 전에 쇠막대기가 끝이나도 총 개수가 늘어난다.  

모든 괄호들은 `Stack`에서 `push`와 `pop`을 하며 관리(?)해주었다. 열린 괄호는 무조건 스택에 넣어주고(아직까진 이 열린 괄호가 쇠막대기의 시작인지 아니면 레이저인지 알 수 없다), 닫힌 괄호는 무조건 스택에서 빼주었다.  

그 다음으로는 괄호가 닫혔을 때, 이게 쇠막대기의 끝인지 레이저인지 구별해준다.  
&#10069;&#10069; 쇠막대기의 끝일 때: 총 개수에 +1을해준다.  
&#10069;&#10069; 레이저일 때: 스택의 크기만큼(열린 괄호의 개수만큼) 총 개수에 더해준다.  

문제에서 주어진 예제로 설명하자면  

![쇠막대기 예제](/img/stackqueue1.png)  

&#10004; 첫 번째 레이저는 이전에 아무런 괄호가 없으므로 첫 번째 레이저를 지나는 쇠막대기가 없다는 뜻이므로 아무런 연산 없이 지나친다.  

&#10004; 두 번째(& 세 번째) 레이저는 현재 스택에 열린 괄호가 3개 들어가 있고, 뒤에 레이저가 있으므로 총 개수에 +3을 해준다.  

&#10004; 네 번째 레이저는 현재 스택에 3개의 열린 괄호가 들어가 있고, 레이저 전에 닫힌 괄호가(쇠막대기의 끝)이 있으므로 총 +4를 해주면 된다.  

이런식으로 하다보면 맨 마지막 레이저까지 쇠막대기의 개수를 구해주면 된다. (맨 마지막 레이저 이후의 쇠막대기 조각은 쇠막대기의 끝을 더해주는 방법과 같다.)  

### 2) 코드
> [전체 코드](https://github.com/soomin0328/Algorithm/blob/master/Algorithm/src/SW_Expert_Academy/%EC%87%A0%EB%A7%89%EB%8C%80%EA%B8%B0%EC%9E%90%EB%A5%B4%EA%B8%B0_5432.java)  
> &#128226; 프로그래머스에서 코딩한게 아니라서 전체적인 로직만 참고하세요!

<br>
```java
for (int j = 0; j < arr.length; j++) {
	if (arr[j].equals("(")) {
		st.push(arr[j]);
	} else {
		st.pop();
                //닫힌 괄호가 레이저인지 쇠막대기의 끝인지 구별
		if (arr[j - 1].equals("(")) {
                    //레이저이면 이 전의 쇠막대기의 개수를 더해줌
		    answer += st.getSize();
		} else {
			answer += 1;
		}
	}
}
```

# 2. 프린터
[문제설명](https://programmers.co.kr/learn/courses/30/lessons/42587?language=java)

### 1) 풀이 과정
인쇄 대기목록의 가장 앞에있는 문서의 중요도보다 높은 것이 존재하면 그 문서는 대기목록의 맨 뒤로 이동하여 어떻게 보면 계속 순환하게 된다. 따라서 이 문제는 FIFO인 큐를 사용하는 것이 적합하다.  

먼저 문서들의 중요도를 넣을 큐와 index를 넣을 큐를 만들어주어, 두 큐에 모든 문서의 중요도와 index값을 넣어준다. 그 다음, 중요도 큐의 맨 앞 요소보다 더 큰 요소가 있는지 확인한다( == 맨 처음 문서의 중요도보다 높은 중요도를 가진 문서가 있는지).  

만약 맨 앞 요소보다 큰 요소가 없다면 인쇄되는 순서 ArrayList에 넣어주고, 있다면 큐의 맨 뒤로 요소를 다시 넣어준다.

### 2) 코드
> [전체 코드](https://github.com/soomin0328/Algorithm/blob/master/Algorithm/src/programmers/StackQueue/%ED%94%84%EB%A6%B0%ED%84%B0.java)  

<br>
```java
for (int i : priorities) {
    queue.offer(i);
    index.offer(queue.size() - 1);
}
        
while (!queue.isEmpty()) {
    int print = queue.poll();
    int printIndex = index.poll();
            
    for (int q : queue) {
        if (q > print) {
            count++;
        }
    }
            
    if (count != 0) {
         queue.offer(print);
        index.offer(printIndex);
    } else {
        arr.add(printIndex);
    }
    count = 0;
}
answer = arr.indexOf(location) + 1;
```
# 3. 다리를 지나는 트럭
[문제설명](https://programmers.co.kr/learn/courses/30/lessons/42583?language=java)

### 1) 풀이 과정
다리를 건너는 트럭과 대기 트럭을 관리할 큐를 따로 만들고, 다리는 정해진 길이와 무게가 있기 때문에 다리를 건너는 트럭의 큐는 weight와 time을 갖는다.  

다리를 건너는 트럭의 큐의 time은 매 시간마다 -1 되고, 만약 time이 1이라면 해당 트럭은 다리를 건너는 큐에서 `poll()`된다. 이 때 다리를 건너는 트럭의 전체 weight에서 해당 트럭의 weight를 빼준다.  

대기하는 트럭이 있고 그 트럭이 다리의 조건(무게 & 길이)에 맞다면 큐에 넣어준다.  

### 2) 코드
> [전체 코드](https://github.com/soomin0328/Algorithm/blob/master/Algorithm/src/programmers/StackQueue/%EB%8B%A4%EB%A6%AC%EB%A5%BC%EC%A7%80%EB%82%98%EB%8A%94%ED%8A%B8%EB%9F%AD.java)  

<br>
```java
class Truck {
	int weight, time;

	Truck(int weight, int time) {
		this.weight = weight;
		this.time = time;
	}

}
```
<br>
```java
total_weight = wait.peek();
q.offer(new Truck(wait.poll(), bridge_length));

while (!q.isEmpty()) {
	if (q.peek().time == 1) {
	    total_weight -= q.poll().weight;
	}

	for (Truck i : q) {
		i.time -= 1;
	}

	if (wait.size() != 0 && total_weight + wait.peek() <= weight) {
		total_weight += wait.peek();
		q.offer(new Truck(wait.poll(), bridge_length));
	}

	time++;
}
```

# 4. 기능개발
[문제설명](https://programmers.co.kr/learn/courses/30/lessons/42586?language=java)

### 1) 풀이 과정
큐에는 각 기능마다 필요한 일 수를 넣어주고, 큐의 맨 앞의 값이 그 다음 값보다 크지 않다면 다음 기능을 기다리지 않아도 되므로 리스트의 해당 index에 1을 넣어준다. 하지만 다음 값보다 크다면 다음 기능의 index에 +1을 해준다.

### 2) 코드
> [전체 코드](https://github.com/soomin0328/Algorithm/blob/master/Algorithm/src/programmers/StackQueue/%EA%B8%B0%EB%8A%A5%EA%B0%9C%EB%B0%9C.java)  

<br>
```java
pre = q.poll();
a.add(1);

while (!q.isEmpty()) {
	if (pre >= q.peek()) {
		a.set(index, a.get(index) + 1);
	} else {
		index++;
		pre = q.peek();
		a.add(1);
	}
	q.poll();
}

int[] answer = new int[a.size()];

int i = 0;
for (int num : a) {
	answer[i] = num;
	i++;
}
```
<br>