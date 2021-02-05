---
layout: page-sidenav
group: "Chapter. 2"
title: "2. Bayes' rule"
author: Hyungjin Chung
---

- **Bayesian inference**
  - Inference: 측량된 확실성을 기반으로 샘플 데이터를 일반화하는 것
  - Bayesian: Inference를 진행할 때 확실성을 측량하는 과정에서 **Bayes' rule**을 이용하는 방식
- 직역하자면 다소 모호하지만 오히려 뒤 예시를 보는 편이 더 직관적이다.
- Bayes' rule은 관측된 데이터 \\( Y = y \\)를 기반으로 우리가 알고싶은 값 \\( H \\)에 대한 probability distribution을 나타내게 해준다.

$$p(H = h|Y = y) \frac{p(H = h)p(Y = y|H = h)}{p(Y = y)} \qquad{(2.1)}$$

$$p(h|y)p(y) = p(h)p(y|h) = p(h,y) \qquad{(2.2)}$$

- 식 (2.1)은 (2.2)로부터 도출되며, 식 (2.2)는 **product rule of probability**이다.
- probability distribution을 도출하고자 하는 대상이 \\( H \\) 이며, 따라서 아무 데이터에도 conditional하지 않은 식 (2.1)의 \\( p(H) \\) 는 \\( H \\) 에 대한 **prior**이다.
- \\( p(Y \| H = h) \\) 는 \\( H = h \\) 인 경우 \\( Y \\) 가 가질 수 있는 값들에 대한 distribution을 의미하며, 이를 **observation distribution** 이라고 한다.
- 위 식을 \\( Y = y \\) 일 때 각각 evaluation할 수 있고, 이 경우에 나오는 \\( p(Y = y \| H = h) \\)를 **likelihood**라고 한다.
- likelihood function은 \\( h \\)에 대한 함수이고, probability distribution이기에 적분값이 1일 필요는 없다.
- 이를 normalize해주기 위해서 \\( p(Y = y) \\)가 분모항에 들어가고, 이를 **marginal likelihood**라고 한다.
- 모든 \\( h \\)값에 대해서 marginalize out 해주기 때문
- 결국 이 모든 것을 고려하면 **posterior distribution**인 \\( p(H = h \| Y = y) \\)를 얻게 된다. 이는 새롭게 observe한 데이터 \\( y \\)를 기반으로 새롭게 업데이트 된 \\( H \\)에 대한 새로운 **belief state**이다.

$$\textrm{posterior} \propto \textrm{prior} \times \textrm{likelihood} \qquad{(2.4)}$$

- summarize하자면 식 (2.4)라고 할 수 있다.
- 이와 같이 Bayes rule을 이용해 unknown value에 대한 belief state를 새롭게 observe된 데이터에 근거하여 업데이트하는 방식을 **Bayesian inference** 또는 **posterior inference**라고 한다.

### 1. Example: testing for COVID-19

- Bayesian inference를 적용하는 아주 대표적인 예시로, 다음을 고려하자.
  - \\( H = 1, 0 \\) 은 각각 코로나에 걸린 상황, 그리고 걸리지 않은 상황을 의미한다.
  - \\( Y = 1, 0 \\) 은 각각 코로나 검사에서 양성이 나온 경우, 그리고 음성이 나온 경우를 의미한다.
- 결국 우리가 원하는 것은 테스트 결과에 따라 **실제로** 코로나에 걸릴 확률이 얼마나 되는지, 그에 대한 posterior distribution \\( p(H = h \| Y = y) \\)이다.
- 검사가 얼마나 정확한지는 아래 table을 통해 정리할 수 있다.

|       | Y = 0       | Y = 1       |
|-------|-------------|-------------|
| H = 0 | TNR = 0.975 | FPR = 0.025 |
| H = 1 | FNR = 0.125 | TPR = 0.875 |

- table을 통해 확인할 수 있는 **sensitivity, specificity** 이외에도 posterior를 구하기 위해 추가로 **prior**를 알아야 하는데, 주어진 예제의 경우 이 prior는 **prevalence**, 즉 병의 발병 확률이다.
- 이 값을 임의로 \\( p(H = 1) = 0.1 \\) 로 설정하자. 이를 통해 posterior probability를 구해보면,

$$
\begin{align}
p(H = 1 | Y = 1) &= \frac{p(Y = 1 | H = 1)p(H = 1)}{p(Y = 1 | H = 1)p(H = 1) + p(Y = 1 | H = 0)p(H = 0)} \\
&= \frac{\textrm{TPR} \times \textrm{prior}}{\textrm{TPR} \times \textrm{prior} + \textrm{FPR} \times (1 - \textrm{prior})} \\
&= \frac{0.875 \times 0.1}{0.875 \times 0.1 + 0.025 \times 0.9} \\
&= 0.795
\end{align}
$$

- 즉, 양성이 나왔을 때 실제로 양성일 확률은 약 79.5%인 것을 알 수 있다.
- 마찬가지 방식으로 음성이 나왔을 때 실제로는 양성일 확률을 구해보면 1.4%가 나온다.

### 2. Example: The Monty Hall problem

- Monty Hall problem은 살면서 한번쯤은 들어봤을 법한, 그러나 그리 직관적이지는 않은 문제이다.

> 1, 2, 3번으로 라벨링 된 세 개의 문이 있다. 세 개의 문 중 하나의 문 뒤에는 큰 차가 상품으로 숨겨져있다. 먼저, 참가자가 하나의 문을 고른다. 사회자는 나머지 두 개의 문 중 차가 있지 않은 문을 열어 참가자에게 보여준다. 그리고 처음 선택한 문이 아닌 다른 하나의 문으로 선택을 바꿀 기회를 준다. 이 때, 참가자는 문을 바꿔야 할까?

- 직관적으로는 문을 바꾸든 말든 확률에는 차이가 없어야 할 것 같다.
- 그러나 실제로는 문을 바꾸는 것이 당첨될 **확률을 두 배 늘리는 것**이 된다.
- 이를 설명하기 위해 Bayes' rule을 이용할 수 있다.
  - \\( H_i \\): 상품이 \\( i \\) 번째 문 뒤에 있는 지
  - \\( Y \\): 몇 번째 문을 사회자가 열었는 지

$$P(H_1) = P(H_2) = P(H_3) = \frac{1}{3} \qquad{(2.11)}$$

- 몇 번째 문에 상품이 있는지에 대한 정보는 알고 있는 바가 없으니 prior는 세 경우 모두 1/3로 동일하다.
- 이제 likelihood를 계산하는 것이 가능한데, 문 1번을 열었다면 사회자가 문 2번 또는 문 3번을 열어야 하므로 옵션은 \\( Y = 2 \\) 또는 \\( Y = 3 \\) 뿐이다.

$$
\begin{align}
P(Y = 2 | H_1) = \frac{1}{2} \quad P(Y = 2 |H_2) = 0 \quad P(Y = 2 | H_3) = 1 \\
P(Y = 3 | H_1) = \frac{1}{2} \quad P(Y = 3 |H_2) = 1 \quad P(Y = 3 | H_3) = 0
\end{align}
$$

- prior과 likelihood가 정해졌으니 posterior를 계산할 수 있는데, 문에 주어진 숫자는 arbitrary하기 때문에 \\( P(H_1 \| Y  = 2) = P(H_1 \| Y = 3) \\) 이다. 따라서 한 경우만을 고려한다.

$$
P(H_i | Y = 3) = \frac{P(Y = 3 | H_i)P(H_i)}{P(Y = 3)} \qquad{(2.13)}
$$

-위 식을 통해 posterior probability를 각각 구하면,

$$
P(H_1 | Y = 3) = \frac{1}{3} , P(H_2 | Y = 3) = frac{2}{3} , P(H_3 | Y = 3) = 0 \qquad{(2.14)}
$$

- 결국 사회자가 선택하지 않은 나머지 한 문으로 선택지를 바꾸는 것이 실제로 확률이 두 배 높다는 것을 확인할 수 있다.

### 3. Inverse problems

- Probability theory에서는 \\( h \\) 라는 state가 있을 때, 이로부터 도출되는 observation \\( y \\)를 예상하고자 한다.
- 이와 반대로, **invserse probability**는 그 반대를 수행하고자 한다.
- 다시 말해, observation \\( y \\) 로부터 실제 state인 \\( h \\) 를 도출하고자 한다.
  - Figure에 나와있는 것처럼, 2d 이미지 하나인 \\( y \\) 로부터 3d shape인 \\( h \\) 를 유추하고자 하는 경우이다.
  - 사실 이것은 상대적으로 extreme한 case이고, 실제로는 위와 같은 문제는 random guessing에 불과하기 때문에, 이보다는 많은 조건이 주어지고 그 조건에 맞는 feasible한 solution을 찾게 된다.
  - 예를 들어, CT 촬영을 할 때 360도 각도에서 모두 촬영을 하면 정확한 image를 reconstruction 할 수 있지만 그의 subset만을 얻었을 경우 완벽한 reconstruction이 어렵다.
  - 이러한 문제들은 알맞은 prior를 이용해서 feasible solution을 찾게 되며, 관련된 대표적인 문제로 *sparse-view CT*, *limited-angle CT* 등이 있다.
- Bayesian perspective에서 말하자면, posterior인 \\( p(h\|y) \\) 를 찾기 위해 prior인 \\( p(h) \\) 와 **forward model** \\( p(y\|h) \\) 를 이용하여 plausible solution을 찾게 된다.
  - 여기서 likelihood가 아닌 forward model이라는 어휘를 쓰는 이유는 inverse problem을 공부하는 사람들이 이런 단어를 쓰기 때문이다.
- Inverse problem이라는 이름에서 알 수 있듯이, 대체로 inverse problem들은 경우의 수가 무한히 많은 **ill-posed problem**이다.
- 더 자세한 내용은 2권에서 다뤄진다고 한다.
