---
created: 2026-03-18T08:52
updated: 2026-03-20T09:33
title: 트랜스포머(Transformer)
---
트랜스포머는 문맥을 고려한 예측을 가능하게 한 모델이다. [[어텐션(Attention)]] 레이어에서 토큰 간의 관계를 계산하고, 피드포워드 네트워크에서 각 토큰에 비선형 변환을 적용하여 토큰 표현을 정교하게 만든다.

Transformer is a model that enables context-aware prediction. It computes relationships between tokens in the attention layer and refines token representations through nonlinear transformations in the feedforward network.

## RNN과 차이

가장 큰 차이점은 문맥을 계산하는 방법에 있다. RNN은 히든 스테이트를 통해 문맥 정보를 순차적으로 전달하는 반면, 트랜스포머는 어텐션을 통해 토큰 간 관계를 병렬적으로 계산하여 문맥을 반영한다.

The biggest difference lies in how they compute context. While RNN sequentially passes contextual information through hidden states, Transformer captures context by computing relationships between tokens in parallel through attention.

## Implementation

```python
import torch
import torch.nn as nn


class TransformerBlock(nn.Module):
    def __init__(self, d_model: int, n_heads: int, mlp_ratio: int = 4, dropout: float = 0.1):
        super().__init__()
        self.norm1 = nn.LayerNorm(d_model)
        self.attn = nn.MultiheadAttention(
            embed_dim=d_model,
            num_heads=n_heads,
            dropout=dropout,
            batch_first=True,
        )
        self.norm2 = nn.LayerNorm(d_model)
        self.ffn = nn.Sequential(
            nn.Linear(d_model, d_model * mlp_ratio),
            nn.GELU(),
            nn.Dropout(dropout),
            nn.Linear(d_model * mlp_ratio, d_model),
        )
        self.dropout = nn.Dropout(dropout)

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        attn_input = self.norm1(x)
        attn_output, _ = self.attn(
            attn_input,
            attn_input,
            attn_input,
            need_weights=False,
        )
        x = x + self.dropout(attn_output)

        ffn_input = self.norm2(x)
        x = x + self.dropout(self.ffn(ffn_input))
        return x


class SimpleTransformer(nn.Module):
    def __init__(
        self,
        vocab_size: int,
        d_model: int = 256,
        n_heads: int = 8,
        n_layers: int = 6,
        max_seq_len: int = 512,
        dropout: float = 0.1,
    ):
        super().__init__()
        self.token_emb = nn.Embedding(vocab_size, d_model)
        self.pos_emb = nn.Embedding(max_seq_len, d_model)
        self.blocks = nn.ModuleList(
            [TransformerBlock(d_model, n_heads, dropout=dropout) for _ in range(n_layers)]
        )
        self.norm = nn.LayerNorm(d_model)

    def forward(self, input_ids: torch.Tensor) -> torch.Tensor:
        batch_size, seq_len = input_ids.shape
        positions = torch.arange(seq_len, device=input_ids.device).unsqueeze(0).expand(batch_size, seq_len)

        x = self.token_emb(input_ids) + self.pos_emb(positions)
        for block in self.blocks:
            x = block(x)

        return self.norm(x)
```

