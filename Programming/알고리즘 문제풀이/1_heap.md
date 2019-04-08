## heap
문제 출처 : <a href = "https://programmers.co.kr/learn/courses/30/lessons/42626">프로그래머스</a>

### heap이란

큐에 우선순위의 개념을 넣은 것이다. 큐(queue)는 선입선출이었지만, 힙은 우선순위가 높을 수록 빨리 나간다. 힙 자료구조는 **완전 이진 트리** 의 일종이며, 최대 힙(max heap)은 부모 노드가 자식 노드보다 값이 크거나 같고, 최소 힙(min heap)은 부모노드가 자식 노드보다 값이 작거나 같다. 힙은 구현할 수도 있으나 대부분의 프로그래밍 언어에서는 기본 자료형으로 들어가거나 빌트인 모듈로 들어가 있다. 파이썬의 경우 **heapq** 라는 이름의 패키지로 기본 탑재되어있다.

### 문제 설명 1

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2) Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다. Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

제한 사항 scoville의 길이는 1 이상 1,000,000 이하입니다. K는 0 이상 1,000,000,000 이하입니다. scoville의 원소는 각각 0 이상 1,000,000 이하입니다. 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다. 입출력 예 scoville K return [1, 2, 3, 9, 10, 12] 7 2 입출력 예 설명 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다. 새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5 가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다. 새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13 가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

### 문제 해결 1

~~~python
# 방법 1. heap 자료구조 대신 기본 list의 sort로 구현. 효율성 테스트 통과 못함.
def solution(scoville, K):
    answer = 0
    while min(scoville)<K:
        # 최소값이 K보다 작은데 원소가 하나 남았다 => 애초에 불가능하다
        if len(scoville)==1:
            return -1
        answer+=1
        scoville.sort()
        first = scoville.pop(0)
        second = scoville.pop(0)
        final = first + 2*second
        scoville.append(final)
    return answer
~~~

~~~python
# 방법 2. 빌트인 패키지 heapq 이용. 하지만 역시 효율성 테스트를 통과 못한다. 이유는 아래 코드
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    while min(scoville)<K:
        if len(scoville)==1:
            return -1
        answer+=1
        first = heapq.heappop(scoville)
        second = heapq.heappop(scoville)
        heapq.heappush(scoville,first + 2*second)
    return answer
~~~

~~~python
# 방법 3. 효율성 테스트 통과. min(scoville)의 시간복잡도를 없앴다.
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    while scoville[0]<K:
        if len(scoville)==1:
            return -1
        answer+=1
        first = heapq.heappop(scoville)
        second = heapq.heappop(scoville)
        heapq.heappush(scoville,first + 2*second)
    return answer
~~~

heap은 이미 순서대로 정렬되어있기 때문에 0번째 애를 빼면 min 값이다(2번째가 2번째 min은 아님). 따라서 min(scoville)을 scoville[0]로 없애줌에 따라 O(N)의 복잡도를 O(1)로 줄일 수 있게 되었다.
