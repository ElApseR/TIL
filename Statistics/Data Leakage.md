# Data Leakage in Machine Learning

*이 글은 <a href="https://machinelearningmastery.com/data-leakage-machine-learning/">Data Leakage in Machine Learning</a>에 저의 사견을 넣어 정리한 글입니다.*

## intro
<img src="https://mblogthumb-phinf.pstatic.net/20160514_249/padi328366_1463234969276YhM6i_JPEG/%B0%EE%BC%BA2.jpg?type=w800">
data leakage는 데이터를 이용한 예측 모델링을 하다 보면 의도치 않게 흔히 발생하는 문제입니다. 예측 모델링 단계에서 우리의 모델은 정해진 training set 이외의 데이터를 보아서는 안되지만, 그 선을 넘는 데이터를 보았을 때 일반적으로 data leakage가 발생했다고 말합니다. 즉, 우리의 모델이 **보지 말아야 할 것을 보았다**는 의미입니다. 이 글에서는 아래의 내용들을 정리합니다.

- 예측 모델링에서 data leakage란 무엇인가
- data leakage가 나타났을 때의 증상 및 그 문제점
- data leakage 문제를 줄일 수 있는 팁

---

## 예측 모델링의 목표

예측 모델링은 우리가 가지고 있는 데이터(train)로부터 만든 모델을 우리가 보지 못한 데이터(test) 상황 하에서도 잘 동작하도록 하는 것이 목표입니다. 이것은 사실 굉장히 어려운 일인데, 우리가 알 수 없는 상황 하에서 우리의 모델이 잘 동작하는지를 평가할 방법이 없기 때문입니다. 그렇기 때문에 우리는 cross-validation 등의 방법을 이용하여 임으로 그러한 상황을 만들어낸 뒤, 우리의 모델을 평가하는 방식으로 모델의 성능을 최적화하곤 합니다.

---

## Data leakage란?

data leakage는 예측 모델을 생성할 때, training set 이외의 정보들이 이용된 경우를 말합니다. 이때 주의해야할 점은, feature engineering 또는 유의미한 variable을 추가해서 넣는 것은, 이론적 뒷받침이 되었다면 모델의 성능을 높이는 데에 큰 도움을 준다는 것입니다. 즉, 여기서 말하는 training set 이외의 정보를 이용한다는 것은, **실제 training 데이터를 얻을 당시의 상황에서 절대 알 수 없는 정보를 넣는다**는 것을 의미합니다. 예를 들어 볼까요.

> 우리는 삼성전자의 주가를 예측하고자 합니다. 우리의 training data는 2년 전부터 한달 전까지의 데이터이고 test data는 최근 한달 동안의 데이터입니다. 이때, 저는 모델의 성능을 높이고자 training set에 한달 전 공개된 삼성전자의 지난 분기 매출액을 column으로 추가해주었습니다. 이것은 2년 전의 우리는 절대 알 수 없는 값입니다. 또, 지난 한 달의 주가는 분명 이 매출액의 영향을 받았을겁니다.

이 예시는 정말 간단하고, 누가 과연 이런 실수를 할까 생각되겠지만 생각보다 데이터 분석 과정에서 매우 흔히 나타나는 실수입니다(물론 좀더 복잡하게 나타나겠지만요). 여기서 확인할 수 있듯이, data leakage는 데이터가 time series적인 특성이 반영된 경우에 흔히 나타납니다.

---

## Data leakage가 발생했는지 알아보기

data leakage가 발생했는지 알아보는 가장 쉬운 방법은 내 모델의 성능을 보는 것입니다. **본인 모델의 예측 성능이 지나치게 좋다면** 이에 감탄하며 주변인들에게 자랑하기 전에 스스로를 의심해보아야합니다. 제 경우 역시 모델의 성능이 지나치게 좋은 경우에 data leakage가 발생한 경우가 많았습니다. data leakage는 특히, 우리가 사용하는 데이터셋이 아래와 같이 복잡한 경우에 자주 발생합니다.

- 위에서 언급한대로, time series적인 특성이 반영된 데이터
    - 위의 예시라면, 해당 시점에서 알 수 있었던 가장 가까운 분기의 매출액을 넣어주는 것이 맞겠죠.
- 랜덤 샘플링 기법을 적용시키기 힘든 그래프 문제(네트워크 그래프를 의미하는 것 같네요)
- 아날로그를 기록한 소리나 이미지 파일(크기와 time stamp가 각 파일마다 따로 존재).
    - 아날로그를 우리가 이용하는 디지털 정보로 바꾸는 것은 결국 sampling이며, 이는 sampling 방식에 따라 그 형태가 달라질 수 있습니다. 아마 글쓴이는 이것 때문에 이러한 얘기를 한 것 같은데 정확히 어떤 부분에서 leakage가 일어나는지는 저도 잘 예상이 되지 않네요..
    
    ---

## Data Leakage를 모델링 과정에서 최소화하기

data leakage는 일반적으로 아래 두 방법으로 최소화할 수 있습니다.

1. data는 cross validation set 각 fold 안에서만 수행하기
2. validation set은 정말 최종 of 최종 확인을 위해서 건드리지 말고 깔끔하게 냅두기

### 1. data는 cross validation set 각각의 안에서만 만지기

(이 항목은 특히 k-fold cross validation을 활용할 때에 포인트가 맞춰진 것 같습니다)  
data leakage는 일반적으로 모델링에 사용될 데이터를 준비하는 과정에서 발생합니다. 이 경우, training data에 대하여 overfitting이 발생하고, 실제 성능은 좋지 않을 것입니다. 예를 들어보겠습니다.

> 우리의 training data의 변수들 전체에 대하여 standardization을 진행한 뒤, 이를 cross validation set으로 분할하여 training과 validation에 사용한다고 가정해보자. 이 방식으로 validation set을 이용하여 모델의 성능을 평가하는 것은 data leakage의 함정에 빠진 것이다.

위의 예시의 경우, 데이터 전체(validation set까지 포함)의 변수들의 분포에 대한 정보가 모델 training 과정에서 활용되는 결과를 낳습니다. 이는 validation set의 정보가 일부 포함되어 있는 것이기 때문에, 해당 예측 모델이 validation set의 정보를 이용하는 것과 마찬가지입니다. 이를 피하는 가장 좋은 방법은 standardization을 k-fold cross validation의 각 fold에 대해 수행하는 것입니다. 즉, feature selection, outlier removal, encoding, feature scaling ,projection methods for dimensionality reduction 등의 작업은 각 fold of cross validation마다 수행해야한다는 것입니다.

### 2. Validation set을 깔끔하게 유지하기

사실 가장 쉬운 방법은 이것이 아닐까 하는데, validation set을 아예 건드리지 않는 것입니다. dataset을 training과 validation으로 나눈 뒤, validation이 존재한다는 사실조차 잊고, training만을 이용하여 예측 모델을 생성합니다. 그리고 모든 게 끝난 뒤 비로소 validation을 이용하여 모델을 평가합니다. 위의 1번 경우처럼 standardization을 해야한다면, traing을 이용해서 구한 parameter(평균, 분산 etc.)를 이용해서 validation도 scaling 해주는 것이 진짜 현실 상황에 맞는 것이겠지요. 사실 1번은 k-fold cross validation 상황에 특정된 것 같고, 이 2번을 기억하는 것이 정말 포인트라고 생각합니다.

---

## Data Leakage를 막을 5가지 팁

1. Temporal Cutoff
    - 우리가 알고자하는 상황의 이전으로만 데이터를 한정시킵니다. 특히, 데이터를 우리가 얻은 시점이 아니라, 해당 데이터가 의미하는 시점을 기준으로 데이터를 맞추는 것에 주의합니다.
2. Add Noise
    - 혹시라도 존재할지 모르는 leaking variable을 방지하기 위하여 데이터에 random noise를 추가해줍니다. 이 경우 모델의 overfitting을 막는 역할도 할 수 있습니다.
3. Remove Leaky Variables
    - 의심되는 변수 딱 하나만 넣고 간단한 예측 모델을 돌려봅니다. 만약 성능이 너무 좋다면 leaky하다고 의심해볼 수 있겠지요. 특히 ID나 계정번호 등의 변수가 leaky할 가능성이 높높습니다.
4. Use Pipelines
    - 위의 k-fold cross validation처럼 데이터를 넣는 데에 같은 과정을 계속 반복하려면 pipiline을 짜는 것이 좋겠죠. 데이터를 항상 정해진 pipeline으로 넣으면 leaky한 변수가 생성되는 것을 막을 수 있을 것입니다. R의 caret 패키지나 scikit-learn의 pipilines를 활용해보세요.
5. Use a Holdout Dataset
    - validation set을 정말 최종 확인을 위해서 전혀 건드리지 말고 유지합니다.

    
