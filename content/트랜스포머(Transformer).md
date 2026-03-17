---
created: 2026-03-18T08:52
updated: 2026-03-18T08:52
title: 트랜스포머(Transformer)
---
트랜스포머는 self-attention을 통해 토큰 간 의존관계를 계산하고, feed-forward network를 통해 각 토큰의 내부 표현을 변환함으로써, 각 토큰이 문맥과 관계 정보를 반영한 표현을 갖도록 만드는 신경망 구조다.

A transformer is a neural network architecture that uses self-attention to model dependencies between tokens and feed-forward networks to transform each token's internal representation, enabling tokens to carry context- and relation-aware representations.

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
