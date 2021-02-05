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
- $$p(Y|H = h)$$ 는 \\( H = h \\) 인 경우 \\( Y \\) 가 가질 수 있는 값들에 대한 distribution을 의미하며, 이를 **observation distribution** 이라고 한다.
- 위 식을 \\( Y = y \\) 일 때 각각 evaluation할 수 있고, 이 경우에 나오는 \\( p(Y = y | H = h) \\)를 **likelihood**라고 한다.
- likelihood function은 \\( h \\)에 대한 함수이고, probability distribution이기에 적분값이 1일 필요는 없다.
- 이를 normalize해주기 위해서 \\( p(Y = y) \\)가 분모항에 들어가고, 이를 **marginal likelihood**라고 한다.
- 모든 \\( h \\)값에 대해서 marginalize out 해주기 때문
- 결국 이 모든 것을 고려하면 **posterior distribution**인 \\( p(H = h|Y = y) \\)를 얻게 된다. 이는 새롭게 observe한 데이터 \\( y \\)를 기반으로 새롭게 업데이트 된 \\( H \\)에 대한 새로운 **belief state**이다.

$$\textrm{posterior} \propto \textrm{prior} \times \textrm{likelihood} \qquad{(2.4)}$$

- summarize하자면 식 (2.4)라고 할 수 있다.
- 이와 같이 Bayes rule을 이용해 unknown value에 대한 belief state를 새롭게 observe된 데이터에 근거하여 업데이트하는 방식을 **Bayesian inference** 또는 **posterior inference**라고 한다.

### 1. Example: testing for COVID-19

- Bayesian inference를 적용하는 아주 대표적인 예시로, 다음을 고려하자.
  - \\( H = 1, 0 \\) 은 각각 코로나에 걸린 상황, 그리고 걸리지 않은 상황을 의미한다.
  - \\( Y = 1, 0 \\) 은 각각 코로나 검사에서 양성이 나온 경우, 그리고 음성이 나온 경우를 의미한다.
- 결국 우리가 원하는 것은 테스트 결과에 따라 **실제로** 코로나에 걸릴 확률이 얼마나 되는지, 그에 대한 posterior distribution \\( p(H = h|Y = y) \\)이다.
- 검사가 얼마나 정확한지는 아래 table을 통해 정리할 수 있다.

|       | Y = 0       | Y = 1       |
|-------|-------------|-------------|
| H = 0 | TNR = 0.975 | FPR = 0.025 |
| H = 1 | FNR = 0.125 | TPR = 0.875 |



