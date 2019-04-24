# Mutual Information(MI)

- 참고
    - <a href="https://en.wikipedia.org/wiki/Mutual_information">wiki Mutual Information</a>
    - <a href="http://sanghyukchun.github.io/62/">SanghyukChun's Blog</a>


## what is Mutual information?
mutual information은 정보이론과 확률이론에서 활용되는 개념으로서 서로 다른 두 개의 random variable(확률변수)에 대하여, 하나의 확률변수가 다른 확률변수에 대해 가지고 있는 **정보량** 이다. 이러한 정보량의 단위는 섀넌의 정보이론의 관점에서 보면 bit로 생각할 수 있다. 정보이론의 기초적인 개념인만큼, 기존의 entropy 및 Kullback–Leibler divergence(KL divergence)와도 크게 연관되어있다. 특히 entropy 관점에서 mutual information을 보면, 어떠한 random variable이 가지고 있는 entropy, 즉 해당 random variable이 나머지 random variable에 대해 가지고 있는 **기대 정보량(expected information amount)** 을 의미하는 것으로 볼 수 있다.  

정보 이론의 관점이 아닌, 확률 이론적인 마인드로 생각해보면, mutual information은 서로 다른 두 variable이 얼마나 mutually dependent한지를 평가하는 개념으로 볼 수 있다. 여기까지만 읽으면 통계의 개념을 아는 사람들은 mutual information이 기존의 correlation과 같은 것인지 헷갈릴 것이다. 간단히 이야기하면, 둘은 찾고자하는 것은 동일하지만 그 방법이 서로 다르며, 서로 반대되는 것이 아닌 상호보완적인 개념이다. correlation은 선형 관련성만 평가하지만 mutual information은 확률적인 관련성을 종합적으로 평가한다. 하지만 (후에 확인하겠지만) Mutual information은 각 random variable의 확률분포를 알아야한다는 단점이 있다. 당연히 correlation은 데이터만 있으면 구할 수 있다. 따라서 이러한 관점에서는 확률변수의 분포를 추정(일반적으로 알려져있지 않으므로)하는 것이 선형 관련성만 측정하는 것보다 불확실성이 더 증가한다고 볼 수도 있다. 따라서 이 둘은 서로 상호보완적으로 동시에 고려하는 것이 좋다고 볼 수 있다.

좀더 통계적으로 보면, p(x,y)라는 joint distribution과 p(x)p(y), 즉 서로가 독립이라고 가정했을 때의 joint distribution 간의 거리를 측정하는 개념으로 생각할 수도 있다.

## related knowledge

- KL divergence
    - 어떠한 **두 확률분포의 차이** (거리와 유사)를 측정하는 개념이다. 일반적으로 우리는 어떠한 확률변수가 따르는 확률 분포를 모르기 때문에, 이를 근사하는 확률 분포를 추정한다. 이렇게 추정된 확률 분포를 실제 확률 분포라 가정하고 이를 이용해 샘플을 추출했을 때 발생되는 정보 엔트로피의 차이를 계산하는 방식이다.
    - KL divergence를 두 확률 분포의 "거리"라고 생각할 수도 있는데, 이는 잘못된 이해이다. 만일 KL divergence가 거리라면, KL(p(x) || q(x)) = KL(q(x) || p(x))이어야하겠지만, 그렇지 않다.
    - 위의 말이 어렵다면, p(x) 라는 실제 분포를 우리가 q(x)로 추정했을 때, 이것이 얼마나 실제 분포와 가깝게 추정되었는지를 평가하는 지표라고 이해하자.
    - KL divergence는 항상 0보다 크거나 같다. 또한 p(x)=q(x)일 때만 0의 값을 가진다.


## 수식으로 보는 mutual Information

markdown은 수식이 써지지 않는 관계로, 그림을 첨부하였다.

<p><a href="https://commons.wikimedia.org/wiki/File:Entropy-mutual-information-relative-entropy-relation-diagram.svg#/media/File:Entropy-mutual-information-relative-entropy-relation-diagram.svg"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Entropy-mutual-information-relative-entropy-relation-diagram.svg/1200px-Entropy-mutual-information-relative-entropy-relation-diagram.svg.png" alt="Entropy-mutual-information-relative-entropy-relation-diagram.svg"></a><br>By <a href="//commons.wikimedia.org/w/index.php?title=User:KonradVoelkel&amp;action=edit&amp;redlink=1" class="new" title="User:KonradVoelkel (page does not exist)">KonradVoelkel</a> - <span class="int-own-work" lang="en">Own work</span>, Public Domain, <a href="https://commons.wikimedia.org/w/index.php?curid=11245361">Link</a></p>

<img src="https://t1.daumcdn.net/cfile/tistory/2232D735553B06F621">  
출처: https://twdml.tistory.com/6 [Toward ML]  
