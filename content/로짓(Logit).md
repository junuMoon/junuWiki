---
created: 2026-03-18T22:31
updated: 2026-03-22T17:04
---
Logit은 모델이 확률을 계산하기 전에 출력하는 실수 범위의 raw score다. 이 값 자체는 확률이 아니며 음수나 양수가 될 수 있고 범위에도 제한이 없다.

Logit is the real-valued raw score produced by a model before it is converted into a probability. It is not itself a probability, can be either negative or positive, and is not bounded to a fixed range.

## Mathematical Definition

이진 분류에서 logit은 확률 $p$를 log-odds로 변환한 값이다.

$$
\text{logit}(p) = \log\left(\frac{p}{1-p}\right)
$$

확률, odds, logit의 관계는 다음과 같다.

- probability: 사건이 일어날 확률 $p$
- odds: 사건이 일어날 확률을 일어나지 않을 확률과 비교한 비율 $\frac{p}{1-p}$
- logit: odds에 로그를 취한 값 $\log\left(\frac{p}{1-p}\right)$

즉, logit이라는 이름은 확률 자체가 아니라 확률을 log-odds로 변환한 값에서 왔다.
딥러닝에서는 이 개념에서 확장되어, sigmoid나 softmax를 적용하기 전의 raw score를 logit이라고 부른다.

In binary classification, logit is the value obtained by converting a probability $p$ into log-odds.

The relationship among probability, odds, and logit is as follows.

- probability: the probability that an event occurs, $p$
- odds: the ratio of the probability that an event occurs to the probability that it does not, $\frac{p}{1-p}$
- logit: the value obtained by taking the logarithm of the odds, $\log\left(\frac{p}{1-p}\right)$

In other words, the term logit comes not from probability itself but from the value obtained by converting probability into log-odds.
In deep learning, this term is extended to refer to the raw score before applying sigmoid or softmax.

![[logit-graph.png]]

## Relation to Sigmoid and Softmax

- sigmoid: 하나의 logit을 하나의 확률로 변환
- softmax: 여러 개의 logits를 확률 분포로 변환

- sigmoid: converts a single logit into a single probability
- softmax: converts multiple logits into a probability distribution
