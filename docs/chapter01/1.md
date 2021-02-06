---
layout: page-sidenav
group: "Chapter. 1"
title: "1. Introduction"
---

### 1.1 머신러닝(machine learning)이란?

> 어떠한 경험 E, 과제 T, 퍼포먼스 척도 P가 있을 때, 컴퓨터 프로그램의 T에 대한 P가 E에 따라 증가하면 프로그램이 배우고 있다고 할 수 있다.

- 본 책에서는 ML을 확률적 관점(probabilistic perspective)에서 바라본다.
  - 다시 말해, 예측해야 할 값, 모델의 파라미터등을 모두 random variable로 판단한다.
  - 이는 불확실성이 있을 때 결정을 내리는 optimal approach
  
### 1.2 Supervised Learning

- Supervised learning에서는 과제 \\(T\\)를 통해 \\(\mathbb{x} \in \mathcal{X} \mapsto \mathbb{y} \in \mathcal{Y} \\)를 학습한다.
- \\(\mathbb{x}\\)는 features, covariates, predictor 등으로 불리며 dimension이 fix 되어있는 vector이다. (예를 들어 image의 pixel)
- output인 \\(\mathbb{y}\\)는 label, target, response 등으로 불리며 마찬가지로 dimension이 fix 되어있다.
- 지도학습에서의 경험 E는 N개의 input-output pair로 주어진다. \\(\mathcal{D} = \{(\mathbb{x}_n, \mathbb{y}_n)\}_{n=1}^N\\)

#### 1.2.1 Classification

- 가장 대표적인 예로 classification에서는 label이 scalar값으로 주어진다. \\(\mathcal{Y} = \{1, 2, \dots, C\}\\).
- 가장 간단한 binary classification인 경우 \\(y \in \{0, 1\}\\) 또는 \\(y \in \{-1, +1\}\\) 로 나타낼 수 있다.
- **Image classification**은 input data가 이미지의 픽셀값이기 때문에 \\(\mathcal{X} = \mathbb{R}^D\\), \\(D = C \times D_1 \times D_2\\)이다.
- 이러한 task를 푸는 데에 convolutional neural network(CNN)이 아주 적합하다.
- 그건 나중에 알아보고, 우선 더 간단한 방식을 고안할 수도 있다.
 
