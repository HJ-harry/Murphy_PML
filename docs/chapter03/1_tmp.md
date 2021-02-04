---
layout: page-sidenav
group: "Chapter. 3"
title: "2. Categorical and multinomial distributions"
---

- 지난 단원에 이어서 label이 binary가 아닌 finite set일 때를 다룬다. 다시 말해, \\( y \in \{1, \dots, C\} \\) 인 경우이다.

### 1. Definition

$$\textrm{Cat}(y|\mathbf{\mu}) := \prod_{c=1}^C \theta_{c}^{\mathrm{I}(y=c)} \qquad{(3.22)}$$

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

$$\textrm{Mu}(\mathbf{s}|N, \mathbf{\mu}) := \begin{pmatrix} N \\ s_{1} \dots s_{C} \end{pmatrix} \prod_{c=1}^C \theta_{c}^{s_c}

### 2. Softmax function

- conditional discriminative model을 생각했을 때, output distribution을 다음과 같이 표현할 수 있다.

$$p(y|\mathbf{x},\mathbf{\theta}) = \textrm{Mu}(\mathbf{y}|1, f(\mathbf{x}; \mathbf{\theta})) \qquad{(3.27)}$$

- 각 class의 확률의 합이 1이어야 하며, 각 component가 0과 1 사이의 값을 가진다는 조건을 충족시켜야 한다.
- binary case에서는 이를 위해 함수 \\( f \\) 를 sigmoid 함수를 이용해 squeeze 했었다.
- 그에 대한 multi-class의 counterpart를 **softmax function**, 또는 **multinomial logit** 이라고 한다.

$$\mathcal{S}(\mathbf{a}) := [\frac{e^{a_1}}{\Sigma_{c'=1}^C e^{a_{c'}}}, \dots , \frac{e^{a_C}}{\Sigma_{c'=1}^C e^{a_{c'}}}] \qquad{(3.28)}$$

- 위 함수는 \\( \mathrm{R}^C \\) 를 \\( [0, 1]^C \\) 로 squeeze해준다.
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
