# 4. Dynamic Programming

Dynamic programming은 MDP가 환경에 대한 perfect model이라고 주어졌을 때, optimal policy를 compute 할 수 있는 알고리즘의 집합이다.
- 가정이 많아서 이론적으로만 주로 사용됨.
- 하지만 대부분의 이 책세어 다루는 알고리즘은 DP와 같이 되기 위해 노력하는 것이므로 중요함
    - 컴퓨터의 튜링머신 같은 느낌이랄까..?
- 키 포인트는 value function을 good policy를 찾는 방법을 구조화하는 데에 사용한다는 것.

## 4.1 Policy Evaluation
- v_π를 임의의 policy pi에 대해 계산하는 방법
    - policy evaliation in the DP literature
    - v_π는 할인률 gamma<1이거나, termination state 존재하면 유일한 해가 존재함이 알려져있음.
- bellman equation의 좌변과 우변이 equal 해질때까지 반복하면, v_pi로 수렴이 될 것이다.
    - bellamn equation이 pi에 대해서 성립하는 것이 알려져있으므로!
    - 이것을 iterative policy evaluation 이라고 함.
    - 이것을 매 iteration step마다 모든 state에 대하여 업데이트 시켜준다.

## 4.2 Policy Improvement
- value function을 찾는 궁극적인 이유는 더 나은 policy를 찾기 위함.
- 어떠한 policy의 state별 value를 구했을 때, 특정 state의 value가 적절한지 알고싶다면
    - 즉, 해당 state에서 이 policy 유지해서 어떤 a  선택하도록 하는 것이 적절한가 궁금하다면?
- state value 와 action value를 비교한다
    - action value가 더 높다면, 해당 state에선 policy 따르지 말아야한다.
    - 즉, 해당 state 에서는 특정 action 취하는 new  policy를 따라야 한다.
    - 이렇게 찾아진 new policy는 기존 policy보다 더 낫다.
- original policy의 value function에 기반하여 greedy 하게 더 나은 policy를 찾는 과정을
    - policy improvement라고 한다.
- 이미 policy가 optimal 하면 더이상 optimal 해질 수 없다.
- 여기선 deterministic 한 policy만 다뤘다(어떤 state에선 1의 확률로 어떤 action 선택)
    - but, stochastic 하게도 확장 가능하다.
    - 동일한 value 주는 action에 0보다 큰 확률 배정, 나머지는 0 배정.
- policy improvement로 나온 policy가 optimal 한 것은 아님. 그저 일반적으로는 ‘개선’된다에 초점

## 4.3 Policy Iteration
- evaluation과 improvement를 반복한다.
    - finite MDP는 finite한 policy 가지므로, 이 iteration은 유한번 내에 수렴한다.

## 4.4 Value Iteration
- policy iteration은 policy evaluation을 포함하므로, iteration 횟수가 많다.
    - v_π에 수렴해야하기 때문이다.
        - 하지만 수렴하기 전에도 할 수 있다.
- policy evaluation 통해 v_π 수렴 전에 잘라서 update해도 policy iteration이 수렴하도록 하는 여러 방법이 있다.
    - 그 중 하나는 policy evaluation을 딱 한 번의 sweep만 하고  improve 하는 것.
        - 이를 value iteration이라고 한다.
- value iteration도 policy iteration처럼 완전한 수렴 하려면 limit(∞)값에 도달해야 한다.
    -  그 전에 일정 값 이하면 iteration 종료한다
- 즉 한 번의 evaluation sweep, 한 번의 improvement sweep을 반복하는 것이다.
    - 이때  evaluation sweep을 여러번 반복하면 더 빨리 수렴한다.
- value iteration도 역시 optimal한 policy를 찾는 것이 목적이 아니다.
    - 주어진 policy에 대하여 greedy 하게 더 나은 policy를 찾는것.
    - 이것 역시도 iterate 해야지 된다.
