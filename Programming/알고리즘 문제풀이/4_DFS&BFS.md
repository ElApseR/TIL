## DFS(깊이 우선 탐색) & BFS(너비 우선 탐색)

<img width=50% src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Small_Network.png">

그래프는 vertex(node)와 edge로 이뤄져있다. vertex는 쉽게 말해 점이고, edge는 이를 잇는 선이다. edge는 방향이 있을 수도 있고, 방향이 없을 수도 있다. 이러한 그래프의 모든 점을 빼먹지 않고 한 번씩 탐사하고 싶은 상황을 가정해보자. 이때 방법은 두 가지이다. 위에부터 순서대로 가는 것과 한 방향으로 끝까지 밀고 간 다음 길을 다시 끝까지 밀고 가는 것이다. 말로는 잘 이해가 안 되니 그림으로 이해하자.

<img src="https://t1.daumcdn.net/cfile/tistory/2254723E588084F830">

- 출처 : <a href="https://mygumi.tistory.com/102">[그래프] DFS와 BFS 구현하기 :: 마이구미</a>

DFS는 구현에서 스택을 활용하고, BFS는 큐를 활용한다. 또한 **인접행렬** 또는 **인접 리스트** 를 활용하여, 각 노드에 연결된 노드의 관계를 저장해두어야한다. 인접행렬은 해당하는 인덱스 간에 연결이 되어있으면 1, 아니면 0이다. 인접 리스트는 해시 자료구조로 생각하면 편한데, 기준이 되는 노드 값을 key로 가지고 value로 해당 노드에 연결된 노드의 리스트를 가진다. 인접 리스트는 따라서 공간 복잡도가 O(V+E)이다. 하지만 인접행렬은 예상대로 공간 복잡도가 O(V*E)로 매우 크다. 이해하긴 편한데 이 부분이 아쉽다. 아래부턴 이를 각각 구현해본다.

### BFS(너비 우선 탐색)

깊이가 1인 애들부터 가장 깊은 애들까지 순서대로 탐색해준다. 위에 말한대로 큐를 활용해서, 깊이가 2인 애들에 연결된 깊이가 3인 애들을 계속 줄을 세운 뒤, 빨리 들어온 애들부터 탐색하는 것이다. 이 구현에서는 인접 리스트를 활용했다.  

아래 코드 구현은 <a href="http://ejklike.github.io/2018/01/05/bfs-and-dfs.html">파이썬을 사용한 그래프의 너비 우선 탐색과 깊이 우선 탐색 구현</a>을 참고했다.

~~~python
graph = {'A': set(['B', 'C']),
         'B': set(['A', 'D', 'E']),
         'C': set(['A', 'F']),
         'D': set(['B']),
         'E': set(['B', 'F']),
         'F': set(['C', 'E'])}

 def bfs(graph, start):
     # 이미 방문한 곳은 제거
     visited = []
     queue = [start]

     while len(queue)>0:
         # 선입선출
         now = queue.pop(0)
         if now not in visited:
             visited.extend(now)

             # set 끼리는 - 연산이 되는 것을 활용한다.
             ## 사실 이 부분은 꼭 필요하진 않는데, if 연산을 줄이기 위해 넣었다.
             queue.extend(graph[now] - set(visited))
     return visited

~~~

### DFS(깊이 우선 탐색)

위에 언급한 대로, 현재 노드에 연결된 노드를 가장 깊이까지 탐색하고 돌아와서 순서대로 탐색하는 것이다. 과거보다 현재를 우선한다고 볼 수도 있을 것 같다. 구현은 위의 BFS에서 queue를 stack으로 바꿔주기만 하면 된다.

~~~python
graph = {'A': set(['B', 'C']),
         'B': set(['A', 'D', 'E']),
         'C': set(['A', 'F']),
         'D': set(['B']),
         'E': set(['B', 'F']),
         'F': set(['C', 'E'])}

 def dfs(graph, start):
     # 이미 방문한 곳은 제거
     visited = []
     queue = [start]

     while len(queue)>0:
         # 후입 선출
         now = queue.pop()
         if now not in visited:
             visited.extend(now)
             # set 끼리는 - 연산이 되는 것을 활용한다.
             queue.extend(graph[now] - set(visited))
     return visited

~~~
