---
created: 2026-03-18T22:02
updated: 2026-03-22T18:12
---
Attention은 각 토큰이 문맥 내 다른 토큰들에 얼마나 주의를 기울일지를 계산하는 메커니즘이다. 입력 시퀀스의 각 토큰은 Query, Key, Value 벡터로 변환되며, 한 토큰의 Query와 다른 토큰들의 Key의 내적을 통해 attention score를 계산한다. 이 score를 [[소프트맥스(Softmax)]]로 정규화해 가중치로 만든 뒤, 이를 Value에 적용해 weighted sum함으로써 문맥이 반영된 새로운 표현을 얻는다. 즉, Attention은 각 토큰이 입력의 어떤 부분에 더 집중해야 하는지를 동적으로 결정하는 방식이다.

Attention is a mechanism that computes how much each token should attend to other tokens in the context. Each token in the input sequence is transformed into Query, Key, and Value vectors, and an attention score is calculated by taking the dot product of one token's Query with the Keys of the other tokens. These scores are normalized with softmax to produce weights, which are then applied to the Values to compute a weighted sum, yielding a new context-aware representation. In short, Attention dynamically determines which parts of the input each token should focus on.

## Q, K, V란?

- Query: 현재 토큰이 찾고 싶은 정보를 나타내는 질의 벡터
- Key: 각 토큰이 어떤 정보를 갖는지 나타내는 색인 벡터
- Value: 각 토큰이 실제로 전달하는 내용을 담은 값 벡터

- Query: a query vector that represents what the current token is looking for
- Key: an index vector that represents what information each token contains
- Value: a value vector that contains the actual content each token passes along

## Formula

$$
Q = XW_Q,\quad K = XW_K,\quad V = XW_V
$$

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V
$$

### Why divide by $\sqrt{d_k}$?

$d_k$가 커질수록 Query와 Key의 내적값은 더 많은 항을 더하게 되므로 함께 커지는 경향이 있다. 이 큰 값을 그대로 softmax에 넣으면 출력 분포가 지나치게 뾰족해질 수 있고, 그 결과 gradient가 작아져 학습이 불안정해질 수 있다. 따라서 $\sqrt{d_k}$로 나누어 score의 크기를 적절한 범위로 조정하면 softmax의 과도한 포화를 막고 학습을 더 안정적으로 만들 수 있다.

As $d_k$ increases, the dot product between Query and Key tends to grow because more terms are summed together. If these large values are passed directly into softmax, the output distribution can become too sharp, which may lead to small gradients and unstable training. Dividing by $\sqrt{d_k}$ scales the scores to a more stable range and helps prevent softmax from becoming overly saturated.

## Implementation

```python
import math
import torch
import torch.nn.functional as F


def scaled_dot_product_attention(query, key, value, mask=None):
    # query: (batch_size, num_heads, seq_len, d_k)
    # key:   (batch_size, num_heads, seq_len, d_k)
    # value: (batch_size, num_heads, seq_len, d_v)
    # mask:  (batch_size, 1, seq_len, seq_len) or compatible shape
    d_k = query.size(-1)

    # scores: (batch_size, num_heads, seq_len, seq_len)
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)

    if mask is not None:
        scores = scores.masked_fill(mask == 0, float("-inf"))

    # attn_weights: (batch_size, num_heads, seq_len, seq_len)
    attn_weights = F.softmax(scores, dim=-1)

    # output: (batch_size, num_heads, seq_len, d_v)
    output = torch.matmul(attn_weights, value)

    return output, attn_weights
```
