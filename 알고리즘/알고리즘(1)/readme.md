# 알고리즘(1)

# 1. 빅오(Big-O) 표기법
빅오(Big-O) 표기법은 알고리즘의 성능을 수학적으로 표현해주는 표기법이다. 이를 이용해 알고리즘의 **시간 복잡도(Time Complexity)** 와 **공간 복잡도(Space Complexity)** 를 나타낼 수 있다.

빅오 표기법은 입력 데이터 값(n)이 아주 큰 값으로 들어온다고 가정을 하기 때문에 아래와 같은 규칙이 가능하다.

* **상수항 무시**
  * n이 아주 큰 값으로 커지게 되면 상수항을 무시할 수 있다.
  * O(2n) -> O(n)
* **최고차항만 표기**
  * 결과에 가장 큰 영향을 미치는 최고차항만 남기고 나머지는 무시할 수 있다.
  * O(3n^2 + 2n + 5) -> O(n^2)
  
## 1-1. 시간 복잡도와 공간 복잡도
### 시간 복잡도(Time Complexity)
시간 복잡도란 빅오 표기법에 대한 시간 개념으로 알고리즘을 수행하는 데에 연산들이 몇 번 이루어지는 지를 숫자로 표현하는 방법이다.

연산의 종류는 산술, 대입, 비교, 이동을 말한다.

### 공간 복잡도(Space Complexity)
공간에 대한 개념으로 알고리즘이 공간을 얼마나 필요로 하는지를 나타낸다. 

크기가 N인 배열을 만든다고 가정하면 공간 복잡도는 O(N)이 되고 NxN인 배열을 만들면 공간 복잡도는 O(N^2)가 된다.

## 1-2. 빅오 표기법의 종류

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/99EF1E395C7EB4B601"></p>

* O(1)
  * 입력 데이터의 크기와 상관없이 언제나 일정한 시간이 걸린다.
* O(logn)
  * 로그는 매우 큰 입력값에서도 큰 영향을 받지 않는다. 이진 탐색의 경우가 여기에 해당한다.
* O(n)
  * 입력 데이터의 크기에 비례해서 알고리즘의 수행시간이 늘어난다. 
* O(nlogn)
  * 병합정렬, 힙정렬 등의 알고리즘에 여기에 해당한다.
* O(n^2)
  * 입력 데이터의 크기(n)의 제곱만큼의 수행시간이 걸리는 알고리즘(버블정렬 등)이 있다.
* O(2^n)
  * 피보나치의 수를 재귀로 구현하는 경우 이러한 수행시간이 걸린다. 

이 외에도 더 많은 표기법들이 있겠지만 대표적인 것들만 정리해봤다.

# 2. 정렬 알고리즘
정렬 알고리즘은 원소들을 번호순이나 사전 순서와 같이 일정한 순서대로 정렬하는 알고리즘이다. 많은 종류의 알고리즘들이 있지만 여기서는 유명한 정렬 알고리즘 몇 개 정도만 정리한다.

## 2-1. 버블 정렬(Bubble Sort)
버블 정렬(거품 정렬)은 1956년에 분석된 아주 오래된 알고리즘으로 그만큼 시간이 오래걸린다는 단점이 있지만 구현은 아주 간단하다.

### 과정
버블 정렬은 서로 인접한 두 원소를 검사하여 정렬하는 방식을 사용한다. 첫 루프를 돌면 가장 큰 원소가 맨 뒤로 이동하므로 두 번째 루프에서는 맨 뒤의 원소는 제외된다.

정렬이 진행되는 과정을 보자

<p align="center"><img src="https://cdn-images-1.medium.com/max/1600/1*ZQmdM7My9QIhvxj98hrweg.gif"></p>

### 시간 복잡도
시간 복잡도는 첫 번째 루프에서 (n-1)번 두 번째 루프에서 (n-2)번... 마지막 루프에서 1번 즉 n(n-1)/2 => **O(n^2)** 의 시간 복잡도가 나온다.

버블 정렬은 정렬이 되어있던 되어있지 않던 2개의 원소를 비교하기 때문에 시간 복잡도는 모두**O(n^2)** 가 나온다.

### 공간 복잡도
주어진 배열 안에서 swap하기 때문에 **O(1)** 의 공간 복잡도를 가진다.

## 2-2. 선택 정렬(Selection Sort)
선택 정렬은 전체 원소들 중에서 기준 위치에 맞는 원소를 선택하여 자리를 교환하는 방식이다.

### 과정
오름차순 기준

주어진 리스트에서 최소값을 찾고 최소값과 맨 앞에 위치한 값과 swap 한다.
그 다음부터는 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

<p align="center"><img src="https://cdn-images-1.medium.com/max/1600/1*to7gYwi5_bkZhx-1kSB0Lg.gif"></p>

### 시간 복잡도
비교횟수는 버블 정렬과 동일하게 n(n-1)/2이므로 **O(n^2)** 의 시간 복잡도를 가진다.

### 공간 복잡도
주어진 배열 안에서 swap하기 때문에 **O(1)** 의 공간 복잡도를 가진다.

## 2-3. 삽입 정렬(Insertion Sort)
삽입 정렬은 1번 인덱스부터 시작하여 현재위치보다 아래쪽을 순회하여 알맞은 위치에 넣어주는 알고리즘이다.

### 과정
두 번째부터 뽑아서 그 숫자가 첫 번째보다 크면 첫 번째 숫자의 오른쪽에, 작으면 왼쪽에 넣는다. 그리고 세 번째 숫자를 뽑아서 앞의 두 숫자와 크기를 비교하고 알맞은 자리에 넣는다.

<p align="center"><img src="https://cdn-images-1.medium.com/max/1600/1*IK3Q4NBRLthllMINV3OxpQ.gif"></p>

### 시간 복잡도
최악의 경우(역으로 정렬되어 있는 경우)에 선택 정렬과 마찬가지로 n(n-1)/2 => **O(n^2)** 의 시간 복잡도를 가진다.

하지만 모두 정렬이 되어 있는 경우에는 한 번씩 비교만 하고 끝나기에 **O(n)** 의 시간 복잡도를 가진다.

즉 최선의 경우는 **O(n)**, 평균과 최악의 경우는 **O(n^2)** 의 시간 복잡도를 가진다.

### 공간 복잡도
주어진 배열 안에서 swap 하기 때문에 **O(1)** 의 공간 복잡도를 가진다.

## 2-4. 합병 정렬(Merge Sort)
합병 정렬은 안정 정렬에 속하며 분할 정복 알고리즘의 하나이다. 

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Merge-sort-example-300px.gif/220px-Merge-sort-example-300px.gif"></p>

**분할 정복 알고리즘**이란
> 그대로 해결하기 어려운 문제를 작운 문제로 분할하여 문제를 해결해나가는 방법

### 과정
하나의 리스트를 두 개의 **균등한** 크기로 **분할**하고 분할된 부분 리스트를 정렬한 다음 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 하는 방법이다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2BCir%2FbtqEamYc2QJ%2FMJ6ky58SnAJk7YxB6e7fk0%2Fimg.png"></p>

두 리스트를 병합할 때는 각 리스트의 첫 번째 요소를 비교하면서 더 작은 원소를 **임시 배열**에 추가한다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcivaGa%2FbtqD9uJcVwO%2Fbv1qKNLdpmLgidoWtvwRt0%2Fimg.png"></p>

### 시간 복잡도
합병 정렬은 이진 트리 형태로 분할하기 때문에 n개의 노드에 대한 이진 트리 높이는 **logn**이 나온다.

크기가 1인 부분 배열 2개를 합병하는 데는 최대 2번의 비교 연산이 필요하고,

각 레벨에서 모든 숫자 n개를 하나씩 읽어가면서 비교해서 병합한다.(n번 읽음) => **O(n)**

따라서 이진 트리의 높이 logn 과 위의 결과를 곱하면 **O(nlogn)** 이라는 시간 복잡도가 나오게 된다.

<p align="center"><img src="https://gmlwjd9405.github.io/images/algorithm-merge-sort/sort-time-complexity-etc.png"></p>

### 공간 복잡도
합병 정렬의 경우 병합할 때 결과를 담아 놓을 임시 배열이 추가로 필요하기 때문에 **O(n)** 의 공간 복잡도를 가진다.

## 2-5. 퀵 정렬(Quick Sort)
퀵 정렬은 불안정 정렬에 속하는 정렬 알고리즘으로 다른 원소와의 비교만으로 정렬을 수행한다.

퀵 정렬은 분할 정복 방법을 통해 리스트를 정렬하는데, 평균적으로는 매우 빠른 수행속도를 자랑한다.

### 과정
* 리스트 안에서 하나의 원소(**피벗, pivot**)를 고른다.
* 피벗 앞에는 피벗보다 작은 값의 원소들이 오고, 뒤에는 피벗보다 큰 값의 원소들이 온다.
  * 피벗을 기준으로 리스트가 두 개로 나누어짐 => 분할
* 나누어진 리스트에 대해 크기가 0이나 1이 될 때까지 위의 과정을 반복한다.

퀵 정렬 알고리즘의 구체적인 과정을 보자면

* 피벗 값을 선택한다. 7
* 현재 리스트의 가장 왼쪽 값을 i, 오른쪽 값을 j로 한다.
* i가 피벗 값보다 크거나 같은 값을 만날 때까지 오른쪽으로 이동한다.
* j는 피벗 값보다 작거나 같은 값을 만날 때까지 왼쪽으로 이동한다.
  * i는 12, j는 2
* i와 j 둘 다 위 조건에 해당하는 값을 만나면 값을 swap 한다.
  * 그림의 3번째 부분(i는 12, j는 2를 만나서 서로 swap 한다.)
* 위 과정을 i의 인덱스가 j보다 클 때까지 반복한다.
  * [1, 2, 5, 7, 3], [14, 7, 26, 12] 에 대해서도 재귀적으로 수행한다.

<p align="center"><img src="https://www.algolist.net/img/sorts/quick-sort.png"></p>

### 시간 복잡도
최악의 경우에는 **O(n^2)** 의 시간 복잡도를 가진다.

만약 나뉜 파티션의 크기가 1과 n-1개로 계속 나누어 진다면 순환 호출의 깊이가 최대 n까지 나올 수 있기 때문에 시간 복잡도가 **O(n^2)** 가 나오게 된다.

<p align="center"><img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/quick-sort-003.png"></p>

평균적으로는 **O(nlogn)** 의 시간 복잡도를 가진다.

파티션이 n/2로 나누어지게 된다면 순환 호출의 깊이가 logn이 나오게 되고 따라서 시간 복잡도는 _logn(순환 호출 깊이) * n(각 순환 호출 단계 연산)_ = **O(nlogn)** 이 나온다.

<p align="center"><img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/quick-sort-002.png"></p>

### 공간 복잡도
주어진 배열 안에서 swap 하기 때문에 **O(1)** 의 공간 복잡도를 가진다.

다른 방식으로 퀵 정렬을 구현하면 공간 복잡도는 달라질 수 있다.

# 3. 재귀 함수(Recursion)
재귀 함수란 어떤 함수에서 자신을 다시 호출하여 작업을 수행하는 방식의 함수를 말한다. 

## 팩토리얼 예제
```js
function factorial(x) {
  if (x < 0) return;
  if (x === 0) return 1;
  return x * factorial(x-1);
}
factorial(3);
// 6
```

이 코드에서 재귀는 이 부분에서 일어난다.
```js
return x * factorial(x-1);
```

코드의 실행 순서를 보면
```js
factorial(3);

// x = 3, if(x === 0) 넘어감 
return 3 * factorial(2);

// x = 2, if(x === 0) 넘어감
return 2 * factorial(1);

// x = 1, if(x === 0) 넘어감
return 1 * factorial(0);

// x = 0,
if(x === 0) return 1;
```
값이 어떻게 나오는지를 보면
```js
factorial(0) 은 1
factorial(1) 은 return 1 * factorial(0) = 1 * 1
facitoral(2) 는 return 2 * factorial(1) = 2 * 1 * 1
factorial(3) 은 return 3 * factorial(2) = 3 * 2 * 1 * 1
// 6 
```

## 피보나치 예제
피보나치 수를 구하는 프로그램도 재귀 알고리즘을 사용해서 구현할 수 있다.

점화식
> f(n) = f(n-1) + f(n-2) (n >= 2)

```js
function fibo(n) {
	if(n <= 1) return n;
	
  return fibo(n-1) + fibo(n-2);
}
fibo(6);
// 8
```

# 참고
* https://jinhyy.tistory.com/9
* https://gyoogle.dev/blog/algorithm/Selection%20Sort.html
* https://ko.wikipedia.org/wiki/%ED%95%A9%EB%B3%91_%EC%A0%95%EB%A0%AC
