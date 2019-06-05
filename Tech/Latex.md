# Latex

## 수식 줄바꾸기
- 참고 <a href="https://www.overleaf.com/learn/latex/Aligning_equations_with_amsmath">overleaf</a>

Latex으로 논문을 작성할 때 긴 수식을 작성하는 경우 자동 줄바꿈이 되지 않고 파일이 깨지는 경우가 발생한다. 이때 해결법을 알아본다.

#### 1. multiline 활용하기

~~~latex
    \begin{multline}
        p(x) = 3x^6 + 14x^5y + 590x^4y^2 + 19x^3y^3\\
        - 12x^2y^4 - 12xy^5 + 2y^6 - a^3b^3
    \end{multline}
~~~

이 경우, 줄 바꿈 된 아래 수식은 오른쪽으로 정렬된다.

#### 2. split 활용하기

일반적으로 논문에 수식을 작성할 때는 여러 개의 수식을 줄줄이 작성하는 경우가 많다. 이때는 보통 *align* 을 활용하는데, 위에 언급한 multiline은 align 안에서는 작동하지 않는다. 따라서 split을 활용한다.

~~~latex
\begin{align}
    \begin{split}
        p(x) ={}& 3x^6 + 14x^5y + 590x^4y^2 + 19x^3y^3\\
                &- 12x^2y^4 - 12xy^5 + 2y^6 - a^3b^3
    \end{split}
\end{align}
~~~

이 경우 수식 전체적으로는 가운데 정렬이 되고, 수식 내부적으로는 **&** 를 기준으로 줄이 맞춰진다.

#### 3. aligned 활용하기

split도 좋은 방법이지만 일반적으로 가장 많이 쓰는 것은 aligned이다. aligned의 예시는 <a href="https://tex.stackexchange.com/questions/44450/how-to-align-a-set-of-multiline-equations">stackexchange</a>에서 가져왔다. phantom은 두 수식의 줄을 맞춰줄 때 '='를 기준으로 맞춰주고 싶은데 좌변의 길이가 서로 달라서 맞춰지지 않는 문제점을 보완해준다. 좌변의 길이가 같다면 사용할 필요가 없으며, 다르다면 가장 긴 좌변의 식을 넣어주면 해당 길이를 기준으로 맞춰준다.

~~~latex
\usepackage{mathtools}

\begin{align}
  \phantom{i + j + k}
  &\begin{aligned}
    \mathllap{a} &= b + c + d\\
      &\qquad + e + f + g + x + y + z
  \end{aligned}\\
  &\begin{aligned}
    \mathllap{i + j + k} &= l + m + n\\
      &\qquad + o + p + q
  \end{aligned}
\end{align}
~~~
