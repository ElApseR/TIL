# GAN 완전 정복

*이 자료는 <a href="https://www.youtube.com/watch?v=odpjk7_tGY0">1시간만에 GAN(Generative Adversarial Network) 완전 정복하기</a>를 정리한 자료입니다.*

### 1. Vanilla GAN
- generative model의 목적
    - 데이터의 probability density function을 잘 모방하여보자!
- 주의사항
    - generator를 backprop할 때, discriminator의 parameter는 학습이 되지 않도록 해야한다
    - generator의 loss function에 실제 수행할 때는 -를 곱해준다
        - 처음에 G가 뱉은 값을 D는 거의 무조건 0이라고 뱉는데, 이때 G의 gradient의 기울기가 너무 작다. 하지만 -를 곱해주면  기울기가 매우 커져서 초반의 cold start 상황을 빠르게 벗어나게 된다.
- gan의 목적함수는 결국 jenson-shannon divergence이다
    - 즉, 목표로 하는 분포와 추정된 분포 사이의 거리를 재는 방법이다.

### 2. DCGAN
- 내용
    - pooling layer를 활용하지 않는다. pooling을 활용하면 unpooling을 할때 blocky한 이미지가 생성되므로 이를 막기 위함이다.
    - 학습의 안정화를 위해서 batch normalization을 활용한다.
    - adam optimizer의 momentum term인 beta를 실험적으로 잘 되는 값인 0.5, 0.99를 활용한다.
        -  convolution layer 4개를 보통 사용하는데, 이때 이 momentum term을 쓰면 학습이 잘 된다.
- 특징
    - latent vertor(z)를 이용해서 산술적인 연산이 가능해진다.
    - word2vec과 유사한 느낌이다.

### 3. LS-GAN(least square gan)
- 내용
    - D의 decision boundary를 생각해보자
    - D의 decision boundary으로부터 매우 멀리 떨어진, 즉 D가 거의 무조건 진짜 데이터일 것이라고 예측하는 G의 output이 있다고 하자. 이 아웃풋은 좋은 것일까? NO
        - 우리는 진짜 데이터랑 비슷한 것을 만드는 게 목표지, D를 잘 속이는 걸 만드는 게 목표가 아니다.
        - 진짜 데이터는 D의 decisinon boundary의 가까이에 위치하고, 1로 분류하는 쪽에 좀 더 많이 몰려있다.
        - 따라서 너무 멀리 떨어진 애들을 진짜쪽으로 당겨오자
    - how?
        - D의 최종 아웃풋으로 sigmoid를 주지 말고, loss function을 cross-entropy loss가 아니라 MSE를 활용하자.
        - 그럼 G의 결과에 대해 D가 너무 좋게 보고, 1 보다도 훨씬 큰 값을 주어도 1에 가깝게 되도록, 오히려 줄이는 방향으로 학습하게 된다.
    - DCGAN보다는 LSGAN이 조금 더 나았던 것 같다.
    - 보통 모델간 평가는 사람이 보고 평가한다
        - inception score등이 있으나 그닥 좋진 않음

### 4. Semi supervised GAN
- 내용
    - D가 진짜/가짜를 구분하는 게 아니라 클래스를 구분하게 된다.
        - MNSIT의 경우, 11개의 class로 구분한다(+fake 클래스)
    - G에는 실제 우리의 데이터가 의미하는 one-hot vector를 concat해서 넣어준다.
        - 일종의 cheating이라고 보면 될 것 같다.
        - 우리가 무엇을 생성할지를 G에게 정확히 지정해줄 수 있다는 의미에서 기존보다 발전되었다.
        - G는 들어간 one-hot vector와 D의 prediction이 정확히 일치하는 게 좋다
    - z vector는 고정이고 onehot vector만 바뀌는 경우 : 스타일은 그대로고 분류되는 기준에 해당하는 것만 바뀐다.
        - ex) 5가지 포즈의 게임캐릭터를 생성하는 과제의 경우, z를 그대로 두고 onehot만 바꾼다면 같은 형태의 캐릭터가 다른 포즈만 취하게 나옴

### 5. ACGAN
- 내용
    - discriminator가 해야할 일이 두 가지다(multi task learning)
        - 해당 input이 real인가 fake인가?
        - 해당 input의 class는 무엇인가?
            - mnist의 경우 10 개의 class
        - 중간까지의 convolution layer는 동일하고, 최종 아웃풋 layer만 하나는 sigmoid, 하나는 softmax
    - G는 SGAN과 동일하게 input, 대신 D와 마찬가지로 output은 multi task
    - 발표자의 개인적인 해석
        - 기존의 GAN은 G에 집중을 했는데 ACGAN은 D의 성능에도 집중을 해준다
        - G가 일종의 data augmentation의 역할을 해줘서, noise가 일정부분 섞인 데이터가 들어오는 경우에도 D가 잘 분류를 하게 해준다.
    - loss function은 두 개의 분류기의 loss function의 합

### 6. Cycle GAN
- 내용
    - 여름을 겨울로, 말을 얼룩말로 바꾸는 gan
    - G는 VAE처럼 convolution으로 사이즈를 줄였다가 늘리는 식으로 얼룩말을 말로 바꿔준다
    - D는 말인지 여부를 학습한다
    - G가 style transfer가 아니라, 완전히 새로운 말을 생성하는 것을 막아주어야한다.
        - transfer된 말 이미지를 다시 VAE 방식으로 해서 얼룩말을 만들어준다.
        - 만들어진 얼룩말 사진을 처음 얼룩말 input과 l2 loss 구해준다
            - reconstruction error가 적게끔 학습된다
    - 저자가 데이터 공개했는데 1만개 정도는 있어야한다.

### 7. Stack GAN
- 내용
    - 텍스트를 주었을 때 그에 맞는 이미지를 생성하고자 한다
    - 기존 gan은 해상도가 떨어진다는 단점이 있었다.
        - 첫번째 stack에서 64-64짜리 저해상도 이미지를 만들고 두 번째 stack input으로 이를 넣어서 128-128짜리 이미지를 만드는 방식으로 해결하자.
    - 주의 : z의 사이즈를 늘린다고 무조건 좋은 결과가 나오지는 않는다.
        - 너무 작아도 안되고 너무 커도 안된다. 일반적으로 128~256 사용한다.

### 8. GAN의 미래
- GAN은 확실한 convergence measure가 존재하지 않는다. 완성된 사진을 보고 사람이 직접 판단해줘야 한다.
    - BEGAN이 measure가 될만한 것을 소개했으나, hyperparameter를 heuristic하게 정해줘야하고, 모델 자체가 엄청 복잡한 경우에만 활용할 수 있다.
    - BATCH Normalization이 아니라 Weight normalization을 쓰는게 낫다는 논문도 있다.
        - 여기서는 convergence measure로 reconstruction loss 활용한다
        - reconstruction loss는 G가 학습이 다 된 상태에서 test set을 잘 만들도록 z를 학습한다.
            - 학습이 다 되면 얼마나 test set을 잘 만들 수 있는지를 확인할 수 있다.
            - 즉, 정확하게 주어져있는 정답을 얼마나 가까이 만들어낼 수 있는가를 보자
                - 모델의 구현력? 수용력?
        - 근데 z를 학습해야한다 -> 배보다 배꼽이 크다
        - 또한 느리다
- Generative model 모두에게 적용되는 문제
    - 더 나은 upsampling 방법을 찾아내야 하지 않을까
        - DCGAN에서 주장된 deconvolution은 checkboard effect가 발생한다.
        - 따라서 resize-convolution을 이용해서 이러한 문제를 줄여줘야한다.
    - 실제로는 BEGAN은 resize convolution이 낫고, 나머지는 deconvolution이 낫다
        - 요즘은 3-3보다 4-4를 활용해서 checkboard pattern을 줄이거나, stride size를 3으로 하는 경우도 있따.
        - 요즘은 4-4를 더 많이 쓰는 추세인 것 같다.
- 기존의 supervised learning을 발전시킬 수 있지 않을까
    - Seq2Seq
        - 주로 machine translation에 활용
        - 한 영어 문장이 여러 방식으로 번역될 수 있지만, seq2seq은 하나의 문장으로만 번역한다.
        - 다양한 방식으로 번역되도록 하고프다! gan을 이용하자!
