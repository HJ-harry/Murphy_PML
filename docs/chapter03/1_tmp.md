---
layout: page-sidenav
group: "Chapter. 3"
title: "2. Categorical and multinomial distributions"
---

- 지난 단원에 이어서 label이 binary가 아닌 finite set일 때를 다룬다. 다시 말해, \\( y \in \{1, \dots, C\} \\) 인 경우이다.

### 1. Definition

$$\textrm{Cat}(y|\mathbf{\mu}) := \prod_{c=1}^C \theta_{c}^{\mathbb{I}(y=c)} \qquad{(3.22)}$$

- 다시 말해, \\( y \\) 가 \\( c \\) 라는 값을 가질 때의 확률을 각각 \\( \theta_c \\) 로 나타낼 수 있다.
- 당연하게도, 아래 두 조건을 충족시켜야 한다.
  - \\( 0 \leq \theta_c \leq 1 \\)
  - \\( \Sigma_{c=1}^C \theta_c = 1 \\)
- practical 하게는 discrete variable인 \\( y \\) 를 **one-hot encoding** 시키는 경우가 대부분이다.
- 이 경우 각각의 class는 \\( (1, 0, 0), (0, 1, 0), (0, 0, 1) \\) 등의 **unit vector**로 encoding 되며, 해당하는 class \\( c \\) 를 제외하고는 0인 값을 갖게 하는 방식으로 encoding이 된다.
- 만약 \\( \mathbf{y} \\) 를 one-hot vector로 고려한다면 categorical distribution은 다음과 같이 나타낼 수 있다.

$$\textrm{Cat}(\mathbf{y}|\mathbf{\mu}) := \prod_{c=1}^C \theta_{c}^{y_c} \qquad{(3.23)}$$

- bionomial distribution이 bernoulli distribution의 generalization이었던 것처럼, categorical distribution의 generalization인 **multinomial distribution**도 존재한다.
- \\( N \\) 번의 categorical trial이 있다고 생각하면, multinomial distribution은 아래와 같이 나타낼 수 있다.

$$\textrm{Mu}(\mathbf{s}|N, \mathbf{\mu}) := \begin{pmatrix} N \\ s_{1} \dots s_{C} \end{pmatrix} \prod_{c=1}^C \theta_{c}^{s_c} \qquad{(3.24)}$$

### 2. Softmax function

- conditional discriminative model을 생각했을 때, output distribution을 다음과 같이 표현할 수 있다.

$$p(y|\mathbf{x},\mathbf{\theta}) = \textrm{Mu}(\mathbf{y}|1, f(\mathbf{x}; \mathbf{\theta})) \qquad{(3.27)}$$

- 각 class의 확률의 합이 1이어야 하며, 각 component가 0과 1 사이의 값을 가진다는 조건을 충족시켜야 한다.
- binary case에서는 이를 위해 함수 \\( f \\) 를 sigmoid 함수를 이용해 squeeze 했었다.
- 그에 대한 multi-class의 counterpart를 **softmax function**, 또는 **multinomial logit** 이라고 한다.

$$\mathcal{S}(\mathbf{a}) := [\frac{e^{a_1}}{\Sigma_{c'=1}^C e^{a_{c'}}}, \dots , \frac{e^{a_C}}{\Sigma_{c'=1}^C e^{a_{c'}}}] \qquad{(3.28)}$$

- 위 함수는 \\( \mathbb{R}^C \\) 를 \\( [0, 1]^C \\) 로 squeeze해준다.
- eq. (3.28) 에 대해 input으로 들어가는 \\( \mathbf{a} = f(\mathbf{x}; \mathbf{\theta}) \\) 는 **logit**이라고 부르며, binary case의 log odds 의 generalization이다.
- sigmoid function이 heaviside step function을 근사하는 데에 이용될 수 있었던 것처럼, softmax function에 temperature를 이용하여 **argmax function** 처럼 behave하게 할 수 있다.
- softmax function의 temperature는 \\( \mathcal{S}(\mathbf{a}/T) \\) 의 \\( T \\) 이며, 이 값이 0에 가까워질수록 argmax function에 근사한다.
- 반대로, \\( T \\) 의 값이 커지면 커질수록 output이 uniform해지는 경향이 있다.

### 3. Multiclass logistic regression

- 함수 \\( f(\dot) \\) 로 linear predictor를 이용하면 최종 모델은 아래와 같다.

$$p(y|\mathbf{x};\mathbf{\theta}) = \textrm{Cat}(y|\mathcal{S}(\mathbf{W}\mathbf{x} + \mathbf{b})) \qquad{(3.30)}$$

- 여기서 얻어지는 \\( \mathbf{a} = \mathbf{W}\mathbf{x} + \mathbf{b}\\) 는 **logit** 값이 되며, 이를 softmax function을 통과시켜 probability를 얻을 수 있다.
- 이를 **multinomial logistic regression** 이라고 칭한다.

### 4. Log-sum-exp trick

- logit을 softmax function에 통과시켜 확률 값을 얻는 데에 numerical problem이 있을 수 있다.

$$p_c = \frac{e^{a_c}}{Z(\mathbf{a})} = \frac{e^{a_c}}{\Sigma_{c'=1}^C e^{a_{c'}}} \qquad{(3.33)}$$

- 분모에 있는 **partition function** \\( Z \\)를 계산할 때, 더해지는 각 항이 ```exp``` 함수이기에 logit 값이 크거나 작으면 overflow/underflow 등이 발생할 수 있다.
  - ```np.exp(1000) = inf```
  - ```np.exp(-1000) = 0```
- 이를 해결하기 위해 아래 property를 이용할 수 있다.

$$\log \sum_{c=1}^C \exp (a_c) = m + \log \sum_{c=1}^C \exp (a_c - m) \qquad{(3.34)}$$

- 위 property는 arbitrary한 \\( m \\) 에 모두 적용되며, 따라서 overflow를 막기 위헤 보통 \\(  m = \max_c a_c \\) 를 이용한다.
- 이러한 property를 이용하는 것을 **log-sum-exp trick** 이라고 부르는데, ```lse``` 함수를 계산하는 데에 쓰이기 때문이다.

$$\textrm{lse}(\mathbf{a}) := \log \sum_{c=1}^C \exp (a_c) \qquad{(3.35)}$$

- 이를 이용하면 logit을 이용해 확률값들을 구할 수 있는데, 방식은 다음과 같다.

$$
\begin{align}
  p_c &= \exp (a_c - \log \sum_{c'=1}^C e^{a_{c'}}) \\
  &= \exp (a_c - \textrm{lse}(\mathbf{a}))
\end{align}
$$

- 이후에 이를 이용해 ```cross-entropy``` loss를 계산 하는 데에 이용할 수 있다.
- 물론 이런 trick 없이 logit으로부터 직접적으로 ```CE loss```를 계산하는 방법도 있다.

$$\mathcal{L} = - [\mathbb{I}(y = 0)\log p_0 + \mathbb{I}(y = 1) \log p_1] \qquad{(3.37)}$$

- 위와 같이 정의된 ```BCE loss```를 계산할 때, 각 항은 아래와 같이 \\( lse() \\) 함수를 이용해 계산할 수 있다.

$$\log p_1 = \log (\frac{1}{1 + \exp (-a)}) = \log (1) - log (1 + \exp (-a)) = 0 - \textrm{lse}([0, -a]) \qquad{(3.38)}$$

$$\log p_0 = \log (\frac{1}{1 + \exp (+a)}) = \log (1) - log (1 + \exp (+a)) = 0 - \textrm{lse}([0, +a]) \qquad{(3.39)}$$


