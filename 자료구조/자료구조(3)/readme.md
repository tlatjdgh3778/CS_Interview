# 자료구조(3)

### 7-6. 레드-블랙 트리(Red-Black Tree)
레드 블랙 트리는 **자가 균형 이진 탐색 트리** 중의 하나로 각각의 노드가 레드나 블랙의 색상을 가지는 이진 탐색 트리이다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Red-black_tree_example.svg/750px-Red-black_tree_example.svg.png"><a href="https://dahye-jeong.gitbook.io/til/algorithm/2018-05-27-algorithm-redblack">https://dahye-jeong.gitbook.io/til/algorithm/2018-05-27-algorithm-redblack</a></p>

이진 탐색 트리가 가지고 있는 조건에 아래와 같은 추가적인 조건을 만족하면 레드 블랙 트리라고 할 수 있다.

* 노드의 색상은 레드 혹은 블랙이다.
* 루트 노드는 블랙이다.
* 모든 리프 노드들은 블랙이다.
  * 여기서의 리프 노드는 NIL 을 의미한다.
* 레드 노드의 자식 노드 양쪽은 언제나 블랙이다.
  * 레드 노드는 연속으로 나올 수 없다는 뜻이다.
  * 레드 노드의 부모는 무조건 블랙이라는 듯이다.
* 모든 리프 노드에서 Balck Depth 는 같다.
  * 루트 노드에서 리프 노드까지 가는 경로에서 만나는 블랙 노드의 개수는 같다.


루트 노드부터 가장 먼 경로까지의 거리가, 가장 가까운 경로까지의 거리의 두 배 보다 항상 작다. 이러한 상태를 균형이 잡혀있다고 한다(balanced). 따라서, 삽입, 삭제, 검색시 최악의 경우(worst-case)에서의 시간복잡도가 트리의 높이(또는 깊이)에 따라 결정되기 때문에 보통의 이진 탐색 트리에 비해 효율적이라고 할 수 있다.

### 삽입
삽입은 이진 탐색 트리에서 하는 것과 같이 노드를 삽입한다. 그 다음 레드 블랙 트리의 조건에 맞게 추가적으로 **색상을 변환(recoloring)** 해주거나 **트리 회전(rotation)** 을 해준다.
새로 삽입되는 노드의 색상은 **레드**이다.

* **부모 노드 (P)와 삼촌 노드 (U)가 레드인 경우**

부모 노드 **(P)** 와 삼촌 노드 **(U)** 를 블랙으로 하고 할아버지 노드 **(G)** 를 레드로 한다. 할아버지 노드 **(G)** 의 부모 노드가 있다면 Double Red가 또 발생될 수 있다. 할어버지 노드 **(G)** 가 루트 노드일 경우에는 블랙으로 한다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/c/c8/Red-black_tree_insert_case_3.png" width="70%">
</p>

* **부모 노드 (P)가 레드이고 삼촌 노드 (U)는 블랙이거나 없는 경우**

**(N)**, **(P)**, **(G)**, 를 오름차순으로 정렬했을 때의 가운데 있는 값을 부모로 만들고 나머지 둘을 자식으로 만든다. 올라간 부모를 블랙으로 하고 자식을 레드로 한다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/6/66/Red-black_tree_insert_case_5.png" width="70%"></p>


#### 20 15 3 12 5 을 삽입해보자.

* 20을 삽입

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/34b9bf60-727a-44fb-80b6-cfc4725fa73f/image.png"></p>

20은 루트 노드이기 때문에 색상을 블랙으로 한다.

* 15를 삽입

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/946d6b7f-0ec0-4802-b8af-be67ee890b82/image.png"></p>

새로 삽입되는 노드는 레드 색상이다.
15는 20보다 작기 때문에 왼쪽 자식으로 들어간다.

* 3을 삽입

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/7781a0d9-e07e-4805-92e9-541987d8acf5/image.png"></p>

3은 20보다 작고 15보다 작기 때문에 15의 왼쪽 자식으로 들어간다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/b0724bca-9f68-4b39-b487-0caed774a32e/image.png"></p>

3의 부모 노드가 레드이고 삼촌 노드가 없이 때문에 오름차순(3, 15, 20) 으로 정렬했을 때 중앙값 15를 부모로 나머지를 자식으로 한다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/5d4ea916-aceb-4eb1-8e68-db87256a9167/image.png"></p>

루트 노드를 블랙으로 변경하고 마지막 조건(루트 노드에서 리프 노드까지 가는 경로에서 만나는 블랙 노드의 개수는 같다.) 을 만족하기 위해 20을 레드로 바꿔준다.

* 12를 삽입

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/ae05b70e-fe73-446b-865d-fafbde9288e8/image.png"></p>

12는 15보다 작고 3보다 크기 때문에 3의 오른쪽 자식 노드로 들어간다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/6516cb8d-5520-4756-b2f7-851f7d98fbc5/image.png"></p>

부모 노드와 삼촌 노드가 레드기 때문에 색상 변환을 해준다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/65ec5e95-c02c-4ebe-ba6d-1f53a061b56a/image.png"></p>

루트 노드는 블랙으로 한다.

* 5를 삽입

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/e9f205b3-9f96-4e95-9f55-725495d850d8/image.png"></p>

5는 15보다 작고 3보다 크며 12보다 작기 때문에 12의 왼쪽 자식 노드로 한다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/e71cefcc-ff2d-4a9a-8a36-ff8bb81cf54f/image.png"></p>

부모 노드가 레드이며 삼촌 노드가 없는 경우이다. (3, 5, 12)의 중간 5를 부모로 하고 나머지를 자식으로 한다. 부모는 검정, 자식은 레드로 만든다.


레드 블랙 트리에 삽입하는 경우 위치를 탐색해야 하기 때문에 **O(logN)** 의 시간복잡도를 가진다.

**삭제**

추가 예정..

### 참고
<a href="https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC#%EC%B2%AB_%EB%B2%88%EC%A7%B8_%EA%B2%BD%EC%9A%B0">https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC#%EC%B2%AB_%EB%B2%88%EC%A7%B8_%EA%B2%BD%EC%9A%B0</a>

* [자료구조 1편 보기](../../자료구조/readme.md)
* [자료구조 2편 보기](../자료구조(2)/readme.md)
