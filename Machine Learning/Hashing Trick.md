# Hashing Trick

- 참고자료
    - <a href="https://ko.coursera.org/lecture/machine-learning-applications-big-data/hashing-trick-GswXH">
Coursera Hashing trick</a>

- 알아볼 것들
    - 해싱 트릭이란 무엇인가
    - 해시 테이블이란 무엇인가?

## Hash table

고정된 사이즈의 array와 hash function을 포함하는 특별한 데이터 타입이다.

### Hash funcition

key값(또는 인덱스)를 어떠한 integer값으로 대응시켜주는 함수이다. 이러한 integer는 fixed array의 위치를 나타낸다. 예를 들어, 자연어 처리의 경우 기존의 방식에서는 각 단어별로 one hot encoding을 한다. 이러한 encoded된 값을 key라고 할 때, 그 키에 맞는 integer를 대응시켜준다(꼭 그런건 아니다 그냥 나의 예시일뿐). text 전처리의 경우 *MurMur3 hash, Jenkins hash, City hash, md5 hash* function이 hash function으로 자주 활용된다.

## Hashing trick이란?

key 값을 hash function의 input으로 하여, hash된 값을 뱉어낸다. Hashing은 주로 feature가 매우 고차원일때(ex. 카테고리 갯수가 너무 많은 범주형 변수) 이들을 저차원으로 줄여주기 위해 사용된다. 하지만 애초에 고차원인 것을 저차원으로 줄여주는 것이기 때문에 서로 다른 input이 hash function에 들어가도 같은 output을 뱉는 경우가 있다. 예를 들어, hash(apple)=hash(pineapple)=10 과 같은 경우가 발생할 수 있는 것이다. 이러한 경우를 **collision** 이라고 한다. collision이 발생하는 경우, fixed size array의 해당 위치에 1씩 더해준다. 예를 들어 위의 경우 fixed array의 10번째 항은 2가 될 것이다. collision은 서로 다른 것을 같은 것으로 취급한다는 점에서 문제이지만, hashing trick이 모델 성능을 올려주기 때문에 감수할만 한 단점이다. 특히 text 데이터의 경우, 기존의 dictionary 방식은 주어진 단어의 index가 무엇인지 dictionary에서 찾아주어야했다. 하지만 hashing trick을 활용하면 단순히 hash function을 계산해주면 된다. 당연한 소리겠지만, hash table의 사이즈가 커질 수록 모델의 성능은 올라간다(collision이 적어져서 원래 데이터를 더 많이 보존하므로). 하지만 그 증가의 형태가 concave한 monotone 증가이므로, 적절한 점을 찾으면 매우 높은 성능을 보장함과 동시에 상당히 작은 사이즈의 hash table을 활용할 수 있다. 이는 메모리 측면에서도 이득이다.

| Dictionary | Hashing Trick     |
| :------------- | :------------- |
| no collisions       | collisions       |
| need to store dictionary for learning  and in production | no dictionary is needed, Calculation on-the-fly(super fast)|
| slow if dictionary is large -> O(log/D/) | Fast -> O(1)|
| feature vector size=unique words and n-grams count | feature vector size=size of the hash table(fixed) |
|variable memory footprint|fixed memory footprint|
