---
created: 2026-03-18T22:27
updated: 2026-03-18T22:39
---
Softmax는 임의의 실수 벡터를 확률 분포로 변환하는 함수다. 각 원소에 지수함수를 적용한 뒤 전체 합으로 나누어, 모든 값이 0과 1 사이에 있고 총합이 1이 되도록 정규화한다. 이를 통해 모델의 raw output인 logit을 "어디에 얼마나 집중할지" 또는 "각 클래스일 확률이 얼마인지"처럼 해석 가능한 가중치로 바꿀 수 있다.

Softmax is a function that converts an arbitrary real-valued vector into a probability distribution. It applies the exponential function to each element and then normalizes by the sum of all elements so that every value lies between 0 and 1 and the total sums to 1. This allows the model's raw outputs, or logits, to be interpreted as meaningful weights, such as how much attention to assign or how likely each class is.


$$
\text{softmax}(x_i) = \frac{e^{x_i}}{\sum_j e^{x_j}}
$$

## Why use exp?

지수함수는 큰 값을 더 크게, 작은 값을 더 작게 만들어 값들 사이의 차이를 더 뚜렷하게 만든다. 동시에 모든 값을 양수로 바꾸기 때문에 전체 합으로 나누어 확률 분포처럼 정규화하기 쉬워진다. 따라서 softmax는 입력값의 상대적인 크기 차이를 유지하면서도 해석 가능한 가중치로 변환할 수 있다.

## Implementation

```python
import torch
import torch.nn.functional as F


logits = torch.tensor([1.2, 0.3, 2.1])
probs = F.softmax(logits, dim=0)

print(probs)        # tensor([0.2558, 0.1040, 0.6402])
print(probs.sum())  # tensor(1.)
```
