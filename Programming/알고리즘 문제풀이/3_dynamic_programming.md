## Dynamic Programming(동적계획법)

동적 프로그래밍이라고 한다. **큰 문제를 잘게 쪼개서** 답을 미리 풀어두고 기억해둔 뒤, 후에 필요한 경우에 다시 계산할 필요 없이 꺼내쓰는 방식의 프로그래밍이다. 분할정복과 헷갈릴 수 있는데, 동적 프로그래밍은 분할 계산 결과가 최대한 많이 재활용되도록 쪼개준다는 특성이 있다. **재활용 프로그래밍이** 라 기억해도 좋겠다. 다양한 문제가 있는데, **0-1 Knapsack Problem** 일명 배낭문제를 풀어보았다.

- **0-1 Knapsack Problem**
    - 배낭에 물건들을 담아야한다. 물건들은 각각 가치(v)와 무게(w)를 가지고 있다. 우리의 배낭은 W만큼의 무게제한이 있다. 따라서 우리의 목표는 물건의 리스트가 주어졌을 때, 배낭에 담을 수 있으며 최대의 가치를 주는 물건의 조합을 찾는 것이다.
    - arg : v(물건들의 가치 리스트), w(물건들의 무게 리스트), Wc(무게제한)
    - return : 가능한 최대 value

~~~python
def solution(v,w,Wc):
    values = np.zeros([len(v)+1,Wc])
    # 행별로(물건별로)
    for i,(val,wei) in enumerate(zip(v,w)):
        # 열별로(무게별로)
        for j in range(Wc):
            #해당 행의 물건을 넣을 수 없는 무게라면 이전에 해당 열에 계산해둔 애를 넣음
            if (j+1) < wei:  
                values[i+1,j] = values[i,j]
            # 넣을 수 있다면, 이전 행의 가치와 (현재 물건 가치 + 이전행에서 남는 무게에 해당하는 가치) 크기 비교해서 더 큰 거 넣음
            else:
                values[i+1,j] = max([val+values[i,j+1-wei], values[i,j]])
    return values[-1,-1]


# example
v = [6,3,5]
w = [3,4,5]
Wc = 10
solution(v,w,Wc) #=> 14
~~~
