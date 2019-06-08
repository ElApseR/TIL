# Missing Value Imputation

missing value는 연습용 데이터가 아닌 현실 데이터에 매우 자주 발생하기 때문에 이에 대한 해결책을 숙지하는 것이 좋다. missing data의 발생 원인을 알아보고, 이에 대한 해결책을 알아보자.

- 참고
    - <a href="https://towardsdatascience.com/how-to-handle-missing-data-8646b18db0d4">How to Handle Missing Data</a>
    - <a href="https:// https://www.youtube.com/watch?v=XnnA9z7lv4Q"> Missing Data Mechanisms(Youtube)</a>

---

## 1. Missing Value 의 발생 원인
- Missing Completely At random(MCAR)
- Missing At Random(MAR)
- Missing Not At Random(MNAR)

### 1-1. MCAR
- 가장 쉬운 경우이다.
- 데이터의 결측값이 완전히 random하게 나타난다. missing rate가 데이터 전반에 따라 동일하다.
    - *P(X1 = miss) = \alpha*
- ex) 도서관 DB에 각 회원별로 책을 연체한 기간이 정리되어 있다. 이때, 데이터를 입력하는 사람이 5%정도 무작위로 실수하여 데이터를 빼먹고 입력하지 안은 상황을 가정해보자. 이것은 데이터 입력 시간이나, 해당 고객의 정보와는 전혀 무관한, 사람의 실수로 인해 발생하는 것이다. 즉, 데이터와 전혀 무관하게 missing value가 발생한다.

### 1-2. MAR
- 데이터의 결측값이 해당 데이터에 존재하는 '다른' 관측값의 영향을 받아 발생한다.
- 가장 쉽게 이해하는 방법은 다른 변수에 conditional하게 missing rate를 특정할 수 있는지 여부이다. MAR의 경우 관측된 데이터에 존재하는 다른 변수값에 의해 missing rate이 달라진다.
    - *P(X1 = miss | X2 = female) = \alpha*
- ex) 지나가는 행인들에게 설문조사를 통해 각자의 성별과 한 달에 책을 몇 권이나 읽는지 설문조사를 하는 상황이다. 이때 여성 행인은 90% 답변하고 남성 행인은 70% 답변한다고 가정하자. 전체 데이터 셋을 보았을 때, 일정 비율의 missing 데이터가 발생한다는 사실은 MCAR과 같다. 하지만, 응답자의 성별을 알면 정확한 missing rate이 얼마인지를 특정할 수 있다는 차이가 있다. 여기서는 성별이라는 단 하나의 변수만 예로 들었으나, 현실에서는 조건이 여러 개가 combine 되어있을 수 있다. 예를 들어, 특정 지역의 특정 나이대의 성별에 따라 missing rate가 다른 경우를 생각해볼 수 있다.
- 즉, 관측된 데이터의 다른 변수값에 의해서 missing rate가 달라지며, 이에 conditional하게 missing rate를 특정할 수 있는 경우를 의미한다.
### 1-3. MNAR
| 독서량  |  결측률 |
|---|---|
| 0  | 80%  |
| 1  | 70%  |
| 2  | 60%  |
| ...  | ...  |
- 가장 보편적으로 많이 발생하는 경우이다.
- ex) 지나가는 행인들에게 설문조사를 통해 각자의 이름과 한 달에 책을 몇 권이나 읽는지 설문조사를 하는 상황이다. 이때, 사람들이 읽은 책 권수가 적을 수록 부끄러워서 답변하는 비율이 적어진다고 가정해보자. 즉, 우리가 궁금한 특정 변수의 missing rate이 해당 변수의 실제 값에 영향을 받는 경우이다.
- MNAR의 가장 큰 문제는 그것이 MNAR인지를 알 수 없다는 것이다. 즉, MNAR인지를 파악하기 위해서는 해당 변수의 실제 값이 missing rate에 영향을 주는지를 파악해야하는데, 실제 값이 얼마인지는 missing이라서 알 수가 없다. 따라서 순환 논증 오류에 빠지게 되는것이다.

---
## 2. Missing Value의 해결

### 2-1. Row Deletion
- 말 그대로 결측값이 존재하는 데이터 행(샘플 하나)를 삭제하는 것이다.
- **장점**
    - 매우매우 간단하다.
- **단점**
    - MCAR의 경우에는 그냥 관측값이 줄어드는 거니까 크게 문제되지 않는다.
    - MAR의 경우에는 conditional한 부분의 고려가 되지 않기 때문에, 이렇게 지운 데이터셋이 실제 데이터셋을 대표할 수 없으며 bias가 발생하게 된다.

### 2-2. Mean/Median Imputation
- 결측치를 해당 column(변수)의 평균 또는 중위값으로 대체하는 것이다.
- **장점**
    - 매우 간단하다.
- **단점**
    - 데이터의 변동성이 줄어든다. 즉, 결측값이 많을 수록 데이터의 분산이 줄어든다.
    - 실제 우리가 최종적으로 활용하는 모델이 각 컬럼의 분산을 계산과정에서 이용한다면 큰 문제가 발생할 것이다.  

### 2-3. Hot deck methods
- 특정 feature의 결측치가 궁금할 때, 현재 관측된 데이터 샘플 내에서 다른 feature들이 비슷한(즉 비슷한 특성을 가진) 데이터의 해당 feature 값으로 대체하는 방법이다. 예를 들어, 키 184, 신체사이즈 x,y,z인 남자의 몸무게가 누락됐을 때, 해당 데이터의 키 184 신체사이즈 x,y,z인 다른 남자의 관측된 몸무게 값으로 채워넣는 것이다. 비슷한 방법으로는 cold deck method가 있는데, 이는 해당 데이터 샘플 외의 데이터에서 비슷한 값으로 대체하는 것이다.
- **장점**
    - 데이터의 더 많은 feature들을 사용하기 때문에 정보가 더 많이 활용된다(즉, 더 educated하다.)
- **단점**
    - 위 두 개의 방법보다 더 computation이 많이 필요하다.

### 2-4. Multiple Imputation

- 위에 나온 세 가지 방법은 모두 single imputation이다. 즉, 세 가지 방법 중 무엇을 쓰든지, 각 결측값에 대하여 딱 하나의 값만 계산된다. 이 경우, 추정된 값에 data의 문제 또는 imputation method 자체의 문제로 인한 bias가 발생할 수 있다. 또한, 우리가 사용한 방법이 해당 상황에 정말 맞지 않는 방법이어도 보정할 수가 없다.
- ex) 각 도서관 회원의 도서관으로부터 거주지의 거리와 도서관 연체료에 대한 데이터가 있다. 연체료 값이 몇 개가 missing인 상황이다. single imputation으로 하면 각 missing value별로 단 하나의 값만 추정될 것이다. 이와 달리, multiple imputation은 single imputation을 여러번 수행하는 것을 의미한다. 예를 들어 hot deck methods로 선형회귀를 이용하며, bootstrap 방식으로 총 1000개의 row 중 50개의 row를 뽑아서 fitting을 한 뒤 imputation을 한다고 하자. 그럼 sample을 뽑을 때마다 회귀선의 회귀 계수가 각각 다르게 추정될 것이고, 그에 따라 이를 종합한 결측값의 추정치가 달라질 것이다. 일반적으로 multiple imputation은 5-10 번의 imputation을 독립적으로 수행한다. 이렇게 각각 수행한 결과 5-10개의 complete dataset이 추정될 것이다. 이를 이용하여 우리가 최종적으로 하고자 했던 계산을 한다. 예를 들어 평균적인 연체료를 계산하는 것이 궁극적인 목표였다면, 5-10개의 평균 연체료가 구해진다. 그 뒤 이를 aggregate(pooling)하여 최종적인 평균 연체료를 구하는데, 예를 들어 aggregate function으로 mean을 사용한다면 최종적인 mean 하나가 나올 것이다.
    - 즉, \hat{mu} = (\hat{mu}_1 + \hat{mu}_2 + ... + \hat{mu}_n)/n
- multiple imputation은 총 세 단계이다.
    0. impute -> analyses -> aggregate(pooling)
    1. single imputation을 각 샘플 데이터셋에 대해 수행한다.
        - 5-10개의 imputed dataset
    2. 도출된 complete dataset을 이용, 최종적으로 하고자 했던 analysis를 수행한다.
        - 5-10개의 추정된 mu
    3. analysis의 결과를 aggregate하여 최종 결과를 도출하고, aggregated 되기 전 애들을 분석해본다. 얘네의 standard deviation을 분석하는 것이 대표적이며, standard deviation이 imputation 단계에서 뽑힌 데이터셋이 대표성을 가지고 있는지, 해당 imputation method가 적절했는지 생각해보아야 한다.
        - 최종적인 하나의 mu

## 3. missing data pattern

1. univariate vs multivariate
    - 데이터의 한 변수에만 결측값이 있는가 여러 변수에 있는가
2. Monotone vs non-monotone 
    - monotone missing의 경우, j번째 열 Y_{j}가 결측이면 Y_{k}(k>j)도 모두 결측인 경우를 의미한다. 주로 longitudinal 데이터에서 특정 시점 이후에는 모두 측정이 되지 않는 경우에 많이 발생한다.
3. Connected vs unconnected
    - Connected하다는 것은 관측된 데이터 포인트가 다른 데이터 포인트에서 위아래 또는 좌우로 이동해서 전부 닿을 수 있는 경우를 의미한다. 즉, 체스의 rook의 움직임을 생각하면 된다.

<img src='/img/missing_patter'>

위 그림에서 모든 경우는 connected이다. connected의 장점은 알려지지 않는 모수를 찾는 것이 가능하다는 것이다. 예를 들어 위에서 file matching의 경우, 2번째와 3번째 열은 서로 correlation을 구할 수 없다. 하지만 전부 관측된 첫 번째 열을 이용하여 우회적으로 correlation을 구할 수 있다.


