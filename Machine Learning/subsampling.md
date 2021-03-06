# Subsampling

데이터를 이용한 모델링에서 Subsampling은 크게 두 가지 용도로 활용된다.

1. 데이터의 크기가 너무 커서 모델의 학습 시간이 너무 오래 걸리거나 리소스가 많이 필요한 경우
2. 불균형 label의 문제로 인해 분류 모델의 성능이 높지 않은 경우
    - 예) 고객이 보험 사기를 칠 것인지 예측하는 모델이 사용하는 데이터가 99.9%의 정상고객과 0.1%의 사기 고객으로 이뤄진 경우

이 중 2번의 경우에 대하여 정상고객(대부분을 차지하는 label)을 r의 비율만큼 subsampling한 뒤 logistic regression 모델을 fitting할 경우 bias가 발생하며 이를 사후적으로 보정해주어야 한다. 이를 유도한 과정은 아래와 같다.

<img src="./img/subsampling.png">
