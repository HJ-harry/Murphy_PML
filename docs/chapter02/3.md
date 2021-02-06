---
layout: page-sidenav
group: "Chapter. 2"
title: "3. Bayesian concept learning"
author: Hyungjin Chung
---

- 지난 섹션에서는 hidden quantity \\( h \in \mathcal{H} \\) 를 단 하나의 observation \\( y \\) 로부터 유추하는 방법을 배웠다.
- 이를 일반화하면 observation이 하나가 아닌 여러 개로 이루어진 *set* 일 때를 생각할 수 있다. \\( \mathcal{D} = \{ y_n : n = 1:N \} \\)
- 이제 목표는 주어진 데이터로부터 보편적인 특성(**concept**)을 찾는 일이다.
- 일반적으로 머신러닝을 통해 binary classification을 학습 시킬 때는 positive example과 negative example이 **둘 다 충분히 많이** 필요하다.
- 반면, 아이가 "강아지" 라는 컨셉을 처음 배울 때는 **positive example**만을 갖고도 효율적으로 학습한다.

### 1. Learning a discrete concept: the number game

- 우리가 어떤 수학적인 컨셉을 배운다고 가정하자. 편의를 위해 몇 가지 assumption이 필요하다.
  - \\( h_{even} = \{ 2, 4, 6, \dots \} \\) 와 같이 컨셉은 **extension**으로 설명된다.
  - 숫자는 1부터 100 사이의 숫자이다.
- \\( \mathcal{D} = \{ 2, 8, 16, 64 \} \\) 가 주어지면, 많은 사람들이 일반적으로 이 set은 2의 승수로 이루어져있다고 판단할 것이다.
- 이는 **generalization**의 한 예시이며, \\( \mathcal{D} = \{ 16 \} \\) 와 같이 데이터 하나만 주어졌을 때보다 데이터가 여러 개 주어졌을 때 더 정확히 일반화할 수 있고, **underlying pattern**
을 더 쉽게 찾을 수 있다.
- 이러한 **induction** problem을 수학적으로 설명하기 위해 hypothesis space \\( \mathcal{H} \\)가 있다고 가정한다.
  - 짝수, 1과 10 사이의 모든 수 와 같은 것이 하나의 *concept*에 해당한다.
- 그 후, hypothesis space \\( mathcal{H} \\) 에 존재하는 concept 중 observed data \\( \mathcal{D} \\)와 consistent하면서, 가장 크기가 작은 subset을 찾는다. 이를 **version space**
라고 칭한다.
- 그러나, version space theory를 이용해도 사람의 행동을 완전히 설명하기는 어렵다. 모든 짝수, 32를 제외한 모든 2의 승수 등의 subset도 모두 observation과 consistent하지만 사람들은능하다.
그러한 concept을 잘 제시하지 않는다. 이러한 현상 또한 bayesian inference로 설명이 가능하다.

#### 1.1 Likelihood

- 왜 사람들은 \\( \mathcal{D} = \{ 2, 8, 16, 64 \} \\) 를 보고 짝수가 아닌 2의 승수를 먼저 떠올릴까?
- **suspicious coincidences**를 막기 위함이다. 실제 \\( h \\) 가 짝수였다면 왜 2의 승수만 관측됐을까?
- 이를 formal하게 표현하기 위해 observation(example)들이 concept의 extension으로부터 랜덤하게 샘플링 되었다고 생각해보자 (이를 **strong sampling assumption**이라 한다.)
- 이 때, unknown concept \\( h \\)로부터 아이템을 \\( N \\)개 sampling 하는 확률은 아래와 같다.

$$
p(\mathcal{D}|h) = \prod_{n=1}^N p(y_n|h) = \prod_{n=1}^N \frac{1}{\textrm{size}(h)}\mathbb{I}(y_n \in h) = [\frac{1}{\textrm{size}(h)}]^N \mathbb{I}(\mathcal{D} \in h) \qquad{(2.16)}
$$

- observation의 모든 데이터 포인트가 \\( h \\)에 존재하여야 \\( \mathbb{I}(\mathcal{D} \in h) \\)가 0이 아닌 값을 가지며, 따라서 probability는 size가 작으면 작을수록 더 probable하다.
- 이를 **size principle**이라고 하고, **Occam's razor**로 설명할 수도 있다.
- \\( \mathcal{D} = \{ 2, 8, 16, 64 \} \\) 일 때 \\( h_{two} \\) 일 확률은 \\( (1/6)^4  7.7 \times 10 \\)
