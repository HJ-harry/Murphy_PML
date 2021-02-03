---
layout: page-sidenav
group: "Chapter. 3"
title: "1. Bernoulli and binomial distributions"
---

- 가장 간단한 probability distribution으로 **Bernoulli distribution**을 들 수 있다.

$$\textrm{Ber}(y|\theta) := \theta^{y} (1 - \theta)^{1-y}  \qquad{(3.1)}$$

- 여기서 \\( 0 \leq \theta \leq 1 \\) 은 \\( y = 1 \\) 인 **확률**이고, Bernoulli distribution으로부터 샘플링할 때 \\( Y ~ \textrm{Ber}(\theta) \\) 와 같이 나타낸다.
- Bernoulli distribution 은 **Binomial distribution**의 special case이다.
- 반대로 말해 Bionomial distribution은 Bernoulli distribution의 generalization인데, trial이 1 보다 클 때, 예를 들어 동전을 여러 번 던졌을 때의 확률이다.
  - N: 동전을 던진 횟수
  - s: 앞면이 나온 횟수
  - \\( \begin{pmatrix} N \\ s \end{pmatrix} \\): \\( N \\) choose \\( k \\) 로 **binomial coefficient** 라고도 부른다.

$$\textrm{Bin}(s|N,\theta) := \begin{pmatrix} N \\ s \end{pmatrix} \theta^s (1 - \theta)^{N-s} \qquad{(3.3)}$$

### 1.2 Sigmoid (logistic) function)

- Bernoulli distribution을 모델링에 쓸 수 있는 대표적인 예를 알아보자.
- \\( \mathbb{x} \in \mathcal{X} \\) 를 input으로 하여 binary class인 \\( y \in \{0, 1\} \\) 를 예측할 때, 아래 **conditional probability distribution**을 사용할 수 있다.

$$p(y|\boldsymbol{x}, \boldsymbol{\theta}) = \textrm{Ber}(y|\sigma(f(\boldsymbol{x};\boldsymbol{\theta})))$$
