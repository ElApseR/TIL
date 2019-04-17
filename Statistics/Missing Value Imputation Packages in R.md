# Missing Value Imputation Packages in R

*지난번에 <a href="https://github.com/ElApseR/TIL/blob/master/Statistics/Missing%20Value%20Imputation.md">Missing Value Imputation</a>을 통해, missing value의 발생 원인과 이를 다루는 다양한 방법들을 알아보았다. 이번에는 missing value imputation에 사용되는 R 패키지들을 알아보도록 하겠다. *

*이 글은 <a href="https://www.analyticsvidhya.com/blog/2016/03/tutorial-powerful-packages-imputing-missing-values/">Tutorial on 5 Powerful R Packages used for imputing missing values</a>를 참고하였다.*

*이 글은 또한 <a href="https://stefvanbuuren.name/Winnipeg/">Handling Missing Data in R with MICE를 참고하였다.</a>*



## 0. Intro
- missing value는 현실 데이터에 자주 발생하며, 많은 black box model들은 이것을 다루는 알고리즘을 내부적으로 가지고 있으나, 해당 모델의 성능에 영향을 끼친다는 것은 의심의 여지가 없다.
- 기존의 row deletion은 정보의 손실이 발생하고, mean 또는 mode로 값을 대체하는 것은 일반적으로 좋지 않다는 것이 알려져있다.
- 따라서 모델 기반의 imputation이 필요하며, R에서는 크게 다섯 가지의 패키지가 유명하다.
    1. MICE
    2. Amelia
    3. missForest
    4. Hmisc
    5. mi


## 1. MICE

**MICE (Multivariate Imputation via Chained Equations)** 는 R을 이용한 imputation을 검색하면 가장 많이 등장하는 패키지이다. MICE는 결측값이 MAR으로 발생했다고 가정한다(사실 앞서 공부했다시피, MNAR 상황부터는 일반화된 모델로 impute 하는 것이 불가능에 가깝다). MAR이므로 당연히 MCAR에도 사용이 가능할 것으로 보인다. MAR은 앞서 보았듯, 다른 관측된 변수에 의존해서 결측값이 발생한 상황이다. 따라서, 다른 관측된 변수를 이용하여 결측값을 impute 하는 것이 논리적으로 가능하다. MICE는 이러한 논리적 기반하에, 다른 변수들을 이용하여, 궁금한 변수를 impute 한다.

> 예) X1,...,Xp 의 변수가 있는 상황에서, X1의 missing value를 X2,...,Xp를 이용한 모델로 impute 하고, X2의 missing value를 X1,X3,X4,...Xp를 이용하여 impute한다. 이때, X1은 앞서 imputation 과정에서 imputed된 값으로 결측값을 대체한 값이다. 즉, Xk를 impute 할 때는 X1,...,X(k-1)은 결측값이 imputed value로 대체된 것을 이용한다. 이렇듯 순서대로 알고리즘을 적용하기 때문에 **Chained** 라는 말이 들어있는 것이다.

MICE의 장점은 impute 하고자 하는 변수의 형태를 반영하여 다양한 imputation model 활용이 가능하다는 것이다. 아래의 표를 참고하자.

|type of variable|model|
|---|---|
|continuous data| predictive mean matching, normal|
|binary data|logistic regression|
|unordered categorical data|polytomous logistic regression|
|ordered categorical data|proportional odds|
|continuous two-level data|normal model, pan, second-level variables|

MICE는 'Fully Conditional Specification(FCS)' 모델을 그대로 따르며, 그 장단점 역시 그대로 받는다.

- 단점
    - 이론적인 근거는 특정 경우에 대해서만 증명되어있다.
    - Sweep Operator처럼 컴퓨팅 시간을 단축하는 알고리즘 사용이 불가하다.   
    - 찾고자하는 joint distribution이 존재하지 않을 수 있다(incompatibility).

- 장점
    - 쉽고 유연하다.
    - 실제 데이터에 근사하게 추정되며, 데이터의 의미적으로 불가능한 추정이 발생하지 않는다.
    - 예측변수의 일부만 사용한다.
    - 모듈 형태로 사용할 수 있어서 안전하다.
    - **정말 잘된다**

MICE는 multiple imputation을 염두에 두고 만들어진 패키지이므로, 몇 번의 imputation을 할지가 중요한데, 일반적으로 5~10번 정도 impute한다. 또한, 저자가 명시하지는 않았으나 chain 형식의 모델이 항상 가지는 문제인, 변수마다 model이 적용되는 순서도 영향이 있을 것으로 보인다(이건 multivariate imputation model이 일반적으로 가지는 문제이기도 하다). 개인적으로, single imputation으로 활용되었을 때, 다른 알고리즘에 비해 성능이 좋은지 궁금하다.
