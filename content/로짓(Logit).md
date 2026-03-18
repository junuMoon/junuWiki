---
created: 2026-03-18T22:31
updated: 2026-03-19T08:25
---
Logit은 모델이 확률을 계산하기 전에 출력하는 raw score다. 이 값 자체는 확률이 아니며 음수나 양수가 될 수 있고 범위에도 제한이 없다. 분류 문제에서는 이 logit에 sigmoid나 softmax를 적용해 최종 확률을 얻는다.

Logit is the raw score produced by a model before it is converted into a probability. It is not itself a probability, can be either negative or positive, and is not bounded to a fixed range. In classification tasks, logits are converted into probabilities by applying sigmoid or softmax.

## Mathematical Definition

이진 분류에서 logit은 확률 \(p\)를 log-odds로 바꾼 값으로도 정의된다.

$$
\text{logit}(p) = \log\left(\frac{p}{1-p}\right)
$$

즉, logit이라는 이름은 확률 그 자체가 아니라 확률을 변환한 값에서 왔다. 다만 딥러닝에서는 보통 모델의 마지막 선형층이 출력한 값을 관용적으로 logit이라고 부른다.

![[logit-graph.png]]

## Relation to Sigmoid and Softmax

- logit: 확률로 바꾸기 전의 raw score
- sigmoid: 하나의 logit을 0과 1 사이 확률로 변환
- softmax: 여러 개의 logit을 전체 합이 1인 확률 분포로 변환
