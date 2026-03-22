---
created: 2026-03-22T17:54
updated: 2026-03-22T17:59
title: RMS놈(RMSNorm)
---
RMS놈(RMSNorm)은 입력 벡터의 평균을 빼지 않고, 크기만 정규화하는 방식이다. 

RMSNorm normalizes only the scale of an input vector without subtracting its mean. 

## Formula

$$
\mathrm{RMS}(x) = \sqrt{\frac{1}{H}\sum_{i=1}^{H} x_i^2}
$$

$$
\mathrm{RMSNorm}(x) = \gamma \odot \frac{x}{\mathrm{RMS}(x) + \epsilon}
$$

## LayerNorm과의 차이

LayerNorm은 평균을 빼고 분산으로 나누지만, RMSNorm은 평균을 빼지 않고 RMS로만 나눈다. 즉 LayerNorm이 중심과 크기를 모두 맞춘다면, RMSNorm은 크기만 맞춘다. 이렇게 평균을 맞추는 과정을 생략하므로 계산이 더 단순하고, 그럼에도 많은 Transformer와 LLM에서 학습 안정성과 성능이 충분히 잘 유지되어 LayerNorm의 대안으로 자주 사용된다.

LayerNorm subtracts the mean and scales by the variance, whereas RMSNorm does not subtract the mean and scales only by the root mean square. In short, LayerNorm normalizes both centering and scale, while RMSNorm normalizes only scale. Because it skips the centering step, RMSNorm is simpler, yet many Transformers and LLMs still maintain good training stability and performance with it, so it is often used as an alternative to LayerNorm.
