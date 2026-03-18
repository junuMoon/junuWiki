---
created: 2026-03-18T22:31
updated: 2026-03-18T23:36
---
Sigmoid는 임의의 실수를 0과 1 사이의 값으로 변환하는 함수다. 입력값이 작을수록 0에 가까운 값을, 클수록 1에 가까운 값을 출력하며, 주로 이진 분류에서 logit을 확률로 변환할 때 사용된다.

$$
\sigma(x) = \frac{1}{1 + e^{-x}}
$$

Sigmoid is a function that maps any real-valued input to a value between 0 and 1. Smaller inputs produce values closer to 0, while larger inputs produce values closer to 1. It is commonly used in binary classification to convert a logit into a probability.

## Interpretation

Sigmoid는 하나의 값을 하나의 확률처럼 바꿔 준다. 따라서 출력값은 어떤 사건이 일어날 가능성, 또는 특정 클래스에 속할 가능성으로 해석할 수 있다.

## Implementation

```python
import numpy as np


def sigmoid(x):
    return 1 / (1 + np.exp(-x))


logit = 2.0
prob = sigmoid(logit)

print(prob)  # 0.8807970779778823
```
