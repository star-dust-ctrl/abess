# Datasets Generator

**abess** includes build-in random sample generators from different distributions in `abess.datasets` module, which can be used for simulation and testing.

- single response: for [gaussian](#linear-regression), [binomial](#logistic-regression), [poisson](#poisson-regression), [gamma](#gamma-regression) and [coxPH](#cox-ph-survival-analysis) model;
- multiple responses: for [multitask](#multitask-regression) and [multinomial](#multinomial-regression) model.

The input can be checked in the [API document](https://abess.readthedocs.io/en/latest/Python-package/datasets/index.html).
The output, whose type is named `data`, contains three elements: `x`, `y` and `coef_`, which correspond the variables, responses and coefficients, respectively. We denote them as $x, y, \beta$ in the math formulas below.

Each row of `x` or `y` indicates a sample and is independent to the other.

## Single Response

The single response data can be generated by `make_glm_data`. Its argument `family` determines what distribution to fit.

### Linear Regression

Usage: `family='gaussian'[, sigma=...]`

Model: $y \sim N(\mu, \sigma^2),\ \mu = x^T\beta$.
- the coefficient $\beta\sim U[m, 100m]$, where $m = 5\sqrt{2\log p/n}$;
- the variance $\sigma = 1$.

### Logistic Regression

Usage: `family='binomial'`

Model: $y \sim \text{Binom}(\pi),\ \text{logit}(\pi) = x^T \beta$.
- the coefficient $\beta\sim U[2m, 10m]$, where $m = 5\sqrt{2\log p/n}$.

### Poisson Regression

Usage: `family='poisson'`

Model: $y \sim \text{Poisson}(\lambda),\ \lambda = \exp(x^T \beta)$.
- the coefficient $\beta\sim U[2m, 10m]$, where $m = 5\sqrt{2\log p/n}$.

### Gamma Regression

Usage: `family='gamma'`

Model: $y \sim \text{Gamma}(k, \theta),\ k\theta = \exp(x^T \beta + \epsilon), k\sim U[0.1, 100.1]$ in shape-scale definition.
- the coefficient $\beta\sim U[m, 100m]$, where $m = 5\sqrt{2\log p/n}$.

### Cox PH Survival Analysis

Usage: `family='cox'[, scal=..., censoring=..., c=...]`

Model: $y=\min(t,C)$, where $t = \left[-\dfrac{\log U}{\exp(X \beta)}\right]^s,\ U\sim N(0,1),\ s=\dfrac{1}{\text{scal}}$ and censoring time $C\sim U(0, c)$.
- the coefficient $\beta\sim U[2m, 10m]$, where $m = 5\sqrt{2\log p/n}$;
- the scale of survival time $\text{scal} = 10$;
- censoring is enabled, and max censoring time $c=1$.

## Multiple responses

The multiple responses data can be generated by `make_multivariate_glm_data`. Its argument `family` determines what distribution to fit.

Note that the output `y` and `coef_` now are both matrix:
- each row of `y` is the responses for a sample;
- each column of `coef_` corresponds to the effect on one response. It is rowwise sparsity. Under this setting, a "useful" variable is relevant to all responses.

### Multitask Regression

Usage: `family='multigaussian'`

Model: $y \sim MVN(\mu, \Sigma),\ \mu^T=x^T \beta$.
- the variance $\Sigma = \text{diag}(1, 1, \cdots, 1)$;
- the coefficient $\beta$ contains 30% "strong" values, 40% "moderate" values and the rest are "weak". They come from $N(0, 10)$, $N(0, 5)$ and $N(0, 2)$, respectively.

### Multinomial Regression

Usage: `family='multinomial'`

Model: $y$ is a "0-1" array with only one "1". Its index is chosed under probabilities $\pi = \exp(x^T \beta)$.
- the coefficient $\beta$ contains 30% "strong" values, 40% "moderate" values and the rest are "weak". They come from $N(0, 10)$, $N(0, 5)$ and $N(0, 2)$, respectively.