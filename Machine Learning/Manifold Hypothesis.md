# Manifold Hypothesis

- references
    - <a href="https://deepai.org/machine-learning-glossary-and-terms/manifold-hypothesis">DeepAI</a>
    - <a href="http://colah.github.io/posts/2014-03-NN-Manifolds-Topology/">colah's blog</a>

## Manifold hypothesis란?

위에 언급한 colah's blog에 따르면 manifold hypothesis의 정의는 아래와 같다

> The manifold hypothesis is that natural data forms lower-dimensional manifolds in its embedding space.

직관적으로 해석했을 때 잘 와닿지가 않는다면 여기서 **embedding** 이라는 단어에 주의할 필요가 있다. 수학에서 어떤 수학적인 구조 X가 같은 형태의 수학적인 구조 Y에 embedded 되어있다고 말하면, 이는 one-to-one 이며 structure-preserving map f:X->Y를 만족함을 의미한다. structure-preserving한 map을 일반적으로 morphism이라고 말하는데, 수업시간에 배웠는데 자세한 건 까먹었다... 그냥 쉽게 어떠한 공간을 상상하고 해당 공간에서의 관계가 그대로 유지되는 것으로 쉽게 이해하고 넘어가자.

그렇다면 manifold란 무엇인가. <a href="https://ko.wikipedia.org/wiki/%EB%8B%A4%EC%96%91%EC%B2%B4">위키피디아</a>를 이용하여 이해해보자. manifold는 위상수학에서 다양체라는 이름으로 불린다. manifold는 국소적으로는 우리에게 익숙한 유클리디안 공간과 닮은 위상 공간이다. 따라서 작게 볼 때는 유클리드 공간과 구분이 되지 않고 유클리드 공간의 성질을 만족하지만, 크게 보면 독특한 위상수학적 구조를 가지며 유클리드 공간의 성질을 만족하지 않는다. 즉, 제한된 조건에서 유클리드 공간과 닮은 공간이다. manifold를 이렇게 정의함으로써 수학자들은 '구', '도넛' 또는 '뫼비우스의 띠'와 같은 독특한 공간도 미적분 같은 기존의 수학 개념을 사용하며 이해할 수 있게 된다. 여기서도 굳이 manifold라는 개념을 쓰는 것은 우리가 앞으로 다룰 공간이 유클리드 공간과 다르게 여러 독특한 형태로 꼬여있을 수 있기 때문이다. 이러한 embedding과 manifold의 정의를 이용하면, 위의 manifold hypothesis의 정의를 이해해볼 수 있다.

**manifold hypothesis는 실생활의 데이터가 그것보다 저차원의 manifold를 형성하며, 이것과 one-to-one하며 structure-preserving한 map을 가진다는 것이다.**

물론 어디까지나 가설이며, 증명된 사실은 아니다. 하지만 이 가설은 <a href="http://www.mit.edu/~mitter/publications/121_Testing_Manifold.pdf">이론적</a>으로 또는 예시를 이용하여 이것이 충분히 논리적이라고 여겨지고 있다. 아래에서는 간단한 예시를 이용하여 manifold 가설이 무엇인지 좀더 직관적으로 이해해보자.

m*n 크기의 흑백 이미지를 분류하는 문제를 푸는 상황을 가정해보자. 각 픽셀은 실수 값을 가지며, 이미지는 의미가 있는 사진부터 완전 랜덤한 noise 까지 섞여있다. 이때, 우리의 데이터는 m*n의 자유도(orthogonal하게 움직일 수 있는 방향)를 가지며, 이에 따라 우리의 이미지는 N=m*n 차원의 공간(manifold) 내의 임의의 한 점이라고 생각할 수 있다. 하지만 만약 우리의 이미지가 아인슈타인을 다양한 각도에서 찍은 사진들이라면 어떨까? 이제는 우리의 이미지가 특정한 의미를 가지므로, N=m*n 차원 공간 내의 임의의 한 점이 아니라 해당 공간의 아주 작은 부분집합일 것이다. manifold hypothesis는 바로 이러한 경우에, 해당 부분집합이 N=m*n 차원보다 훨씬 작은 차원의 (둘러쌓여진) 공간에 위치할 것이라고 주장한다.

그렇다면 manifold 가설이 머신러닝에서 왜 중요할까? manifold hypothesis는 머신러닝 모델이 주어진 데이터에서 주요한 feature를 골라내고, 높은 성능의 prediction을 수행할 수 있는 이유를 설명한다. 우리에게 주어지는 실제 데이터는 매우 많은 feature를 가지고 있다. 하지만, 실제로 우리에게 필요한 key feature의 갯수는 이보다 적을 것이다(factor analysis의 목적을 생각해보자). 머신 러닝 모델은 이러한 핵심 feature만 이용하면 잘 학습되겠으나, 이 핵심 feature들은 보통 주어진 모든 feature들로 이뤄진 복잡한 함수로써, 직접적으로 접근하기가 힘들다. 따라서 많은 머신러닝 모델의 기본적인 아이디어는 이러한 복잡한 embedding function을 잘 풀어내는 것이 핵심이다. 
