---
layout: page-sidenav
group: "Chapter. 3"
title: "1. Bernoulli and binomial distributions"
---

- 가장 간단한 probability distribution으로 **Bernoulli distribution**을 들 수 있다.

$$\textrm{Ber}(y|\theta) := \theta^{y} (1 - \theta)^{1-y}  \qquad{(3.1)}$$

- 여기서 \\( 0 \leq \theta \leq 1 \\) 은 \\( y = 1 \\) 인 **확률**이고, Bernoulli distribution으로부터 샘플링할 때 \\( Y \tilde \textrm{Ber}(\theta) \\) 와 같이 나타낸다.
- Bernoulli distribution 은 **Binomial distribution**의 special case이다.
- 반대로 말해 Bionomial distribution은 Bernoulli distribution의 generalization인데, trial이 1 보다 클 때, 예를 들어 동전을 여러 번 던졌을 때의 확률이다.
  - N: 동전을 던진 횟수
  - s: 앞면이 나온 횟수
  - \\( \begin{pmatrix} N \\ s \end{pmatrix} \\): \\( N \\) choose \\( k \\) 로 **binomial coefficient** 라고도 부른다.

$$\textrm{Bin}(s|N,\theta) := \begin{pmatrix} N \\ s \end{pmatrix} \theta^s (1 - \theta)^{N-s} \qquad{(3.3)}$$

### 1.2 Sigmoid (logistic) function)

- Bernoulli distribution을 모델링에 쓸 수 있는 대표적인 예를 알아보자.
- \\( \boldsymbol{\mathbb{x}} \in \mathcal{X} \\) 를 input으로 하여 binary class인 \\( y \in \{0, 1\} \\) 를 예측할 때, 아래 **conditional probability distribution**을 사용할 수 있다.

$$p(y|\boldsymbol{\mathbb{x}}, \boldsymbol{\theta}) = \textrm{Ber}(y|f(\boldsymbol{\mathbb{x}};\boldsymbol{\theta})) \qquad{(3.5)}$$

- 이 때 \\( f(\boldsymbol{\mathbb{x}};\boldsymbol{\theta})) \\)는 Bernoulli distribution의 mean을 예측하는 ambiguous한 함수이고, 이에 관한 내용은 추후에 다룬다.
- 함수 \\( f \\) 는 결국 probability를 예측해야 하기에 \\( 0 \leq f \leq 1 \\) 이라는 제약이 붙고, 이에서 좀 더 자유로워지기 위해 **sigmoid** 또는 **logistic** 함수를 이용한다.

$$p(y|\boldsymbol{\mathbb{x}}, \boldsymbol{\theta}) = \textrm{Ber}(y|\sigma(f(\boldsymbol{\mathbb{x}};\boldsymbol{\theta}))) \qquad{(3.12)}$$
$$\sigma(a) := \frac{1}{1 + e^{-a}} \qquad{(3.13)}$$

-이 때 편의를 위해 \\( a = f(\boldsymbol{\mathbb{x}};\boldsymbol{\theta}) \\) 를 정의하며, 이를 **log odds** 라고 한다.
-log odds는 다음과 같이 정의되며, 이를 이용해 여러가지 용이한 식을 얻을 수 있다.

$$\log{(\frac{p}{1 - p})} = \log{(\frac{e^a}{1 + e^a} \frac{1 + e^a}{1})} = \log{(e^a)} = a \qquad{(3.17)}$$

$$p(y = 1|\boldsymbol{\mathbb{x}}, \boldsymbol{\theta}) = \frac{1}{1 + e^{-a}} = \sigma(a) \qquad{(3.15)}$$
$$p(y = 0|\boldsymbol{\mathbb{x}}, \boldsymbol{\theta}) = 1 - \frac{1}{1 + e^{-a}} = \sigma(-a) \qquad{(3.16)}$$

- **logit function**은 \\( p \\) 를 log-odds인 \\( a \\) 로 매핑시키며, **logistic function** 은 그 역을 수행한다.

$$\textrm{logit}(p) := a \qquad{(3.18)}$$
$$\textrm{logistic}(a) := p \qquad{(3.19)}$$

- 책에 수식이 잘못 나와있는 듯 하다.

### 1.3 Binary logistic regression

- logistic function이 \\( f \\) 의 range에 무관하게 값을 \\( [0, 1] \\) 로 squeeze 시켜주기 때문에, 0.5인 값을 **decision boundary**로 이용해서 binary classification problem을 푸는 데에 이용할 수 있다.
- 책에서는 \\( f(\boldsymbol{\mathbb{x}};\boldsymbol{\theta})) = \boldsymbol{\mathbb{w}}^T \boldsymbol{\mathbb{x}} \\) 와 같은 linear model을 이용하며, 실제로 간단하게 많이 씅니다.


