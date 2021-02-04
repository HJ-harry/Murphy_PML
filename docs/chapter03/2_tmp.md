---
layout: page-sidenav
group: "Chapter. 3"
title: "3. Univariate Gaussian (normal) distribution"
---

-본 단원에서는 **Gaussian distribution(normal distribution)**을 다룬다.

### 1. Cumulative distribution function

- continuous random variable \\( Y \\)가 있을 때, 이에 대한 **cumulative distribution function(cdf)**는 다음과 같이 정의된다.

$$P(y) := \textrm{Pr}(Y \leq y) \qquad{(3.40)}$$

- 위 함수를 이용하면, 특정 interval 내의 확률값을 계산할 수 있다.

$$Pr(a < Y \leq b) = P(b) - P(a) \qquad{(3.41)}$$

- Gaussian의 cdf를 고려하기 전, **error function**을 먼저 알아보자.
  - \\( z = (y - \mu)/\sigma \\)
  
$$\textrm{erf}(u) := \frac{2}{\sqrt{\pi}} \int_0^u e^{-t^2}dt \qquad{(3.43)}$$

- error function은 standard normal distribution의 mean인 0으로부터 \\( u \\) 까지 적분한 면적이다.
- 식 (3.43)을 이용해서 Gaussian의 cdf를 계산할 수 있고,

$$\Phi(y;\mu,\sigma^2) := \int_{-\infty}^y \mathcal{N}(z|\mu,\sigma^2)dz = \frac{1}{2}[1 + \textrm{erf}(z/\sqrt{2})] \qquad{(3.42)}$$

- 파라미터 값들은 다음과 같다.
  - \\( \mu \\): 평균
  - \\( \sigma^2 \\): 분산
  - \\( \lambda = 1/\sigma^2 \\): precision
- \\( Y \\) 의 cdf가 \\( P \\) 라고 할 때, cdf의 값이 \\( q \\) 를 갖게 하는 \\( y_q \\) 를 \\( q' \\)th **quantile**이라고 한다.
- \\( \Phi \\) 가 standard normal distribution의 cdf라고 할 때, 이의 역함수인 \\( \Phi^{-1} \\) 를 **probit function**이라고 한다.

### 2. Probability density function
- **probability density function(pdf)**는 cdf를 미분한 함수로 정의한다.

$$p(y) := \frac{d}{dy}P(y) \qquad{(3.45)}$$

- Gaussian의 pdf는 다음과 같다. (앞에 곱해지는 \\( \sqrt{2\pi\sigma^2} \\)) 항은 pdf의 총 적분값이 1이 되도록 normalize해주는 constant이다.

$$\mathcal{N}(y|\mu,\sigma^2) := \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{1}{2\sigma^2}(y - \mu)^2} \qquad{(3.46)}$$

- 또한, 특정 interval 내의 확률값은 식 (3.41)을 통해 구할 수 있다고 했는데, \\( a = y, b = y + dy \\)로 두면

$$\textrm{Pr}(y \leq Y \leq y + dy) \approx p(y)dy \qquad{(3.48)}$$

- 결국에 아주 작은 interval내의 면적 값은 pdf 값에다 interval의 width를 곱한 값이다.
- 따라서 width가 매우 작다면 pdf의 한 점에서의 값은 1보다 클 수 있음을 의미한다.
- pdf를 통해 **mean(expected value)** 와 **variance**도 정의된다.
- Gaussian의 경우 mean 값이 \\( \mu \\)로 간단하게 나오지만, 다른 pdf 중 finite integral을 갖지 않는경우 mean 값이 정의되지 않을 수도 있다.

$$\mathbb{E}[Y] := \int_{\mathcal{Y}} y p(y) dy \qquad{(3.49)}$$

$$
\begin{align}
  \mathbb{V}[Y] &:= \mathbb{E}[(Y - \mu)^2] = \int (y - \mu)^2 p(y) dy \\
  &= \int y^2 p(y) dy + \mu^2 \int p(y) dy - 2\mu \int y p(y) dy = \mathbb{E}[Y^2] - \mu^2
\end{align}
$$

$$\mathbb{E}[Y^2] = \sigma^2 + \mu^2 \qquad{(3.52)}$$

### 3. Regression

- 위에서는 Gaussian distribution을 확률분포 그 자체로만 고려했고, unconditional이었다.
- 만약 Gaussian dist. 의 파라미터가 특정 input에 conditional한 경우를 고려할 수 있다.

$$p(y|\mathbf{x};\mathbf{\theta}) = \mathcal{N}(y|f_\mu(\mathbf{x};\mathbf{\theta}), f_\sigma(\mathbf{x};\mathbf{\theta})^2) \qquad{(3.54)}$$

- 식 (3.54)는 가장 일반적인 경우로, \\( f_\mu, f_\sigma \\) 가 각각 mean과 variance를 estimate한다.
- **Homoscedastic regression**
  - Variance는 fix해두고 mean만 conditional하게 알고 싶은 경우
$$p(y|\mathbf{x};\mathbf{\theta}) = \mathcal{N}(y|\mathbf{w}^T \mathbf{x} + b, \sigma^2)$$
  - 얻어지는 모델은 **linear regression** 모델이다.
- **Heteroskedastic regression**
  - Variance도 input-dependent하게 하고 싶은 경우
$$p(y|\mathbf{x};\mathbf{\theta}) = \mathcal{N}(y|\mathbf{w}_{\mu}^{T} \mathbf{x} + b, \sigma_{+}(\mathbf{w}_\sigma^T \mathbf{x}))$$
  - mean과는 다르게 variance는 양수여야 한다는 조건 때문에 softplus 함수를 이용한다.
  
$$\sigma_{+}(a) = \log (1 + e^a) \qquad{(3.57)}$$

- 모델의 uncertainty를 바라보는 관점은 여러가지 있을 수 있다.
- \\( [\mu(x) - 2\sigma(x), \mu(x) + 2\sigma(x)] \\) 는 *observation* \\( y \\) 의 variability를 나타내는 confidence interval이다.
- mean값을 예측하는 \\( f_\mu \\) 자체의 variability도 \\( \sqrt{\mathbb{V}[f_\mu(\mathbf{x};\mathbf{\theta})]} \\)로 나타낼 수 있는데, 이 때의 uncertainty는 파라미터 \\( \mathbf{\theta} \\)에 대한 것이다.

### 4. Why is the Gaussian distribution so widely used?

1. 오직 두 개의 parameter - mean/variance 만 존재하며, 이를 해석하기 용이하다.
2. **central limit theorem**은 independent random variable의 합이 gaussian distribution을 따른다는 것을 보이며, 이 때문에 noise를 모델링하기 용이하다.
3. Maximum entropy를 갖는다 (추후에 논의된다)
4. 수학적으로 간단한 form을 갖는다. (Exponential family의 대표적인 case)

### 5. Half-normal

- non-negative real variable에 대한 distribution을 모델링 하고 싶을 때 한 가지 방법으로 gaussian distribution의 음수부를 양수부쪽으로 접은 모양의 distribution을 생각할 수 있다.
- 이를 **half-normal distribution**이라고 하고, 다음과 같다.

$$\mathcal{N}_{+}(y|\sigma) := 2\mathcal{N}(y|0, \sigma^2) = \frac{\sqrt{2}}{\sigma\sqrt{\pi}} \exp (-\frac{y^2}{2\sigma^2}) \quad y \geq 0 \qquad{(3.58)}$$

$$\textrm{mean} = \frac{\sigma\sqrt{2}}{\sqrt{\pi}}, \textrm{var} = \sigma^2(1 - \frac{2}{\pi}) \qquad{(3.59)}$$

