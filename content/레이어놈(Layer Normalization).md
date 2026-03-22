---
created: 2026-03-22T17:17
updated: 2026-03-22T17:46
title: 레이어 정규화(Layer Normalization)
---
레이어놈(Layer Normalization)은 한 샘플 내부의 feature 값들에 대해 평균과 분산을 이용해 정규화한 뒤, 학습 가능한 스케일과 시프트를 적용하는 연산이다. 이를 통해 입력 분포를 안정화하여 학습을 더 원활하게 만들 수 있다.

Layer Normalization is an operation that normalizes feature values within a single sample using their mean and variance, then applies learnable scale and shift parameters. This helps stabilize the input distribution and makes training easier.

## Formula

입력 벡터를 $x = (x_1, x_2, \dots, x_H)$라고 하면,
$$
\mathrm{LayerNorm}(x) = \gamma \odot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta
$$

- $H$: 정규화하는 차원 수
- $\mu$: 한 샘플 내부 feature들의 평균
- $\sigma^2$: 한 샘플 내부 feature들의 분산
- $\epsilon$: 0으로 나누는 것을 막는 작은 값
- $\gamma, \beta$: 학습 가능한 scale, shift 파라미터

- $H$: number of dimensions being normalized
- $\mu$: mean of the features within a single sample
- $\sigma^2$: variance of the features within a single sample
- $\epsilon$: small constant to prevent division by zero
- $\gamma, \beta$: learnable scale and shift parameters

## BatchNorm과의 차이

BatchNorm은 배치 차원에서 평균과 분산을 계산하고, LayerNorm은 각 샘플 내부 feature 차원에서 평균과 분산을 계산한다. 따라서 BatchNorm은 "같은 배치 안 여러 샘플을 함께 보고 정규화해도 괜찮은" 구조에서 주로 사용된다. 대표적으로 CNN처럼 배치 안 샘플들이 비슷한 통계 구조를 공유하고, 채널 단위 feature map을 함께 정규화해도 학습이 잘 되는 경우가 이에 해당한다.

BatchNorm computes mean and variance across the batch dimension, while LayerNorm computes them across the feature dimension within each sample. Therefore, BatchNorm is often used in architectures where it is acceptable to normalize several samples together, such as CNNs, where channel-wise feature maps across samples tend to share similar statistical structure.

## Implementation

```python
import torch

def layer_norm(x: torch.Tensor, gamma: torch.Tensor, beta: torch.Tensor, eps: float = 1e-5):
    mean = x.mean(dim=-1, keepdim=True)
    var = ((x - mean) ** 2).mean(dim=-1, keepdim=True)
    x_hat = (x - mean) / torch.sqrt(var + eps)
    return gamma * x_hat + beta


x = torch.randn(2, 4, 8)   # [batch, seq_len, hidden]
gamma = torch.ones(8)
beta = torch.zeros(8)
W = torch.randn(8, 8)
b = torch.randn(8)

ln_x = layer_norm(x, gamma, beta)
y = ln_x @ W + b

print(ln_x.shape)  # torch.Size([2, 4, 8])
print(y.shape)     # torch.Size([2, 4, 8])
```
