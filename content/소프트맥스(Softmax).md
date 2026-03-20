---
created: 2026-03-18T22:27
updated: 2026-03-20T09:36
---
Softmax는 실수 벡터를 확률 분포로 바꾸는 함수다. 보통 logit을 클래스별 확률이나 attention 가중치로 해석할 때 사용한다.

Softmax converts a real-valued vector into a probability distribution. It is commonly used to interpret logits as class probabilities or attention weights.


$$
\text{softmax}(x_i) = \frac{e^{x_i}}{\sum_j e^{x_j}}
$$

## Why use exp?

exp는 값을 양수로 바꿔 확률로 만들 수 있게 하며, 더 큰 score에 더 큰 확률을 주도록 차이를 강조한다.

The exponential function makes all values positive so they can be turned into probabilities, while also emphasizing differences so larger scores receive larger probabilities.

## Implementation

```python
import numpy as np


def softmax(x):
    exp_x = np.exp(x)
    return exp_x / np.sum(exp_x)


logits = np.array([1.2, 0.3, 2.1])
probs = softmax(logits)

print(probs)        # [0.25582435 0.1040533  0.64012235]
print(probs.sum())  # 1.0
```
