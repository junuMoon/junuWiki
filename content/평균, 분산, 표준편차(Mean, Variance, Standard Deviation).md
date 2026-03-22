---
created: 2026-03-20T09:45
updated: 2026-03-22T17:05
---
평균, 분산, 표준편차는 데이터의 중심과 퍼짐 정도를 나타내는 기본 통계량이다. 평균은 데이터의 대표값이고, 분산과 표준편차는 데이터가 평균에서 얼마나 퍼져 있는지를 나타낸다.

Mean, variance, and standard deviation are basic statistics that describe the center and spread of data. The mean represents a typical value, while the variance and standard deviation describe how far the data is spread around the mean.

## 평균 (Mean)

평균은 데이터 값을 모두 더한 뒤 개수로 나눈 값이다.

The mean is the value obtained by adding all data points and dividing by the number of points.

$$
\mu = \frac{1}{n}\sum_{i=1}^{n} x_i
$$

## 분산 (Variance)

분산은 각 데이터가 평균에서 얼마나 떨어져 있는지를 제곱해 평균낸 값이다.

Variance is the average of the squared distances between each data point and the mean.

$$
\sigma^2 = \frac{1}{n}\sum_{i=1}^{n}(x_i - \mu)^2
$$

## 표준편차 (Standard Deviation)

표준편차는 분산에 제곱근을 취한 값이다. 분산보다 원래 데이터와 같은 단위를 가져 해석이 더 직관적이다.

Standard deviation is the square root of the variance. Because it has the same unit as the original data, it is often easier to interpret than variance.

$$
\sigma = \sqrt{\sigma^2}
$$

## Relation

- 평균: 데이터의 중심
- 분산: 평균에서 얼마나 퍼져 있는지
- 표준편차: 분산을 원래 단위로 다시 나타낸 값

- mean: the center of the data
- variance: how spread out the data is around the mean
- standard deviation: variance expressed again in the original unit
