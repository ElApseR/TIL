## 스택/큐
문제 출처 : <a href = "https://programmers.co.kr/learn/courses/30/lessons/42583">프로그래머스</a>

### 스택/큐?

스택은 동전 탑을 쌓고 이것을 다시 분해하는 것과 마찬가지로 늦게 올린 동전을 먼저 빼야 하는, 즉 늦게 넣은 데이터가 먼저 나오는(후입선출) 자료형이다. 큐는 지하철에서 줄을 설 때 먼저 선 사람이 먼저 나오듯이, 먼저 들어간 데이터가 먼저 나오는(선입선출) 자료형이다.

### 문제 설명 1 - 다리를 지나는 트럭

트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

|경과 시간 |	다리를 지난 트럭|	다리를 건너는 트럭|	대기 트럭|
|---|---|---|---|
|0	|[]|	[]	|[7,4,5,6]|
|1~2	|[]	|[7]	|[4,5,6]|
|3	|[7]	|[4]	|[5,6]|
|4	|[7]	|[4,5]	|[6]|
|5	|[7,4]	|[5]	|[6]|
|6~7	|[7,4,5]	|[6]	|[]|
|8	|[7,4,5,6]|	[]	|[]|
따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

제한 조건
bridge_length는 1 이상 10,000 이하입니다.
weight는 1 이상 10,000 이하입니다.
truck_weights의 길이는 1 이상 10,000 이하입니다.
모든 트럭의 무게는 1 이상 weight 이하입니다.
입출력 예

|bridge_length|	weight|	truck_weights|	return|
|---|---|---|---|
|2|	10|	[7,4,5,6]	|8|
|100|	100|	[10]|	101|
|100|	100	|[10,10,10,10,10,10,10,10,10,10]|110|

~~~python
def solution(bridge_length, weight, truck_weights):
    answer = 0
    complete_trucks = []
    running_trucks = [0 for i in range(bridge_length)] # [0,0,0]

    while truck_weights:
        # 한대 더 올라갈 수 있는 경우
        if weight >= truck_weights[0] + sum(running_trucks[1:]):
            # 앞으로 한 칸 밀고 뒤에 붙여줌
            running_trucks.pop(0)
            running_trucks.append(truck_weights.pop(0))
            answer += 1
        # 한대 더 못 올라가는 경우
        else:
            # 앞으로 한 칸 밀어줌
            running_trucks.pop(0)
            running_trucks.append(0)
            answer += 1

    # 대기중인 애는 없지만 아직 달리는 마지막 애가 있는 경우
    while sum(running_trucks):
        running_trucks.pop(0)
        running_trucks.append(0)
        answer += 1

    return answer
~~~

단, 이 풀이는 저의 개인 풀이로서 시간 복잡도가 매우 높습니다. 다른 풀이로 푸는 것이 좋을 것 같습니다.
