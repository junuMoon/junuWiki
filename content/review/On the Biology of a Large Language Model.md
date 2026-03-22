---
title: On the Biology of a Large Language Model
created: 2025-09-22T00:00
updated: 2026-03-22T20:56
---
- https://transformer-circuits.pub/2025/attribution-graphs/biology.html
- Anthropic가 Claude 3.5 Haiku 내부를 attribution graph로 뜯어본 글.
- 내가 읽은 느낌으로는 "feature를 봤다"보다 "feature 사이 경로를 보기 시작했다"가 더 핵심.
- 제목을 biology라고 붙인 것도 좀 과하다고 생각했는데 읽고 나면 납득은 됨.
- 세포 찾기에서 끝나는 게 아니라 회로 비슷한 걸 보려는 시도라서.

이 글은 해석가능성 글 중에서도 꽤 재밌었다. 왜냐면 막연히 "모델 안에 뭔가 있다"가 아니라, 진짜로 중간 계산이 어떻게 이어지는지 보여주려 하기 때문. 물론 다 보여주는 건 아니고, 오히려 못 보는 것도 되게 솔직하게 말한다.

> "The black-box nature of models is increasingly unsatisfactory."

약간 이런 문제의식에서 시작하는 글.

## Method Overview?

- replacement model을 만든다.
- MLP neuron 대신 더 해석 가능한 feature를 두고, 그 feature들 사이 attribution graph를 뽑는다.
- attention은 거의 고정값 취급.
- 그래서 이건 full explanation은 아니고 local explanation에 가깝다.
- 그래도 그냥 activation atlas보다 훨씬 재밌다. 적어도 path를 보니까.

- error node도 둔다.
  - replacement model이 원본을 못 따라간 부분.
  - 이게 있다는 게 좋았음. "우린 다 설명했다"가 아니라 "여긴 아직 dark matter다"라고 말하는 거니까.

좀 웃긴 건 여기서부터 이미 biological metaphor가 먹힌다는 점.
세포를 찾는 게 아니라 wiring diagram을 그리고 싶다는 얘기.

## 제일 재밌었던 것들

> We present a simple example where the model performs “two-hop” reasoning “in its head” to identify that “the capital of the state containing Dallas” is “Austin.” We can see and manipulate an internal step where the model represents “Texas”.

- Dallas -> Texas -> Austin
  - 이건 거의 데모 느낌.
  - "Dallas가 있는 주의 수도"를 답할 때 내부에서 Texas를 거친다는 그림.
  - chain-of-thought를 안 써도, 안에서는 `in its head` 몇 단계 reasoning이 있을 수 있다는 것.
  - 이런 건 늘 반쯤 의심하면서 읽게 되는데, 여기선 꽤 설득력 있었다.

- Planning in Poems
  - 이 부분 좋음.
  - 시 한 줄을 쓰기 전에 line ending 후보를 먼저 잡아두고, 거기에 맞게 앞부분을 채운다는 식.
  - `rabbit`, `habit` 같은 rhyme candidate를 먼저 본다는 얘기인데, 이건 그냥 다음 토큰 예측기라는 느낌보다 훨씬 계획적이다.
  - "즉흥처럼 보이는 출력도 내부에서는 미리 끝을 보고 쓴다"는 주장.

> We find the model uses a mixture of language-specific and abstract, language-independent circuits. The language-independent circuits are more prominent in Claude 3.5 Haiku than in a smaller, less capable model.

- Multilingual circuits
  - 언어별 회로가 따로 있는 것도 맞는데, 그 아래엔 언어를 덜 타는 추상 회로도 있다는 쪽.
  - 여기서 살짝 universal mental language 같은 톤이 나오는데, 좀 과장 같으면서도 또 재밌다.
  - 적어도 표면 언어를 넘어서 `language-independent circuits` 비슷한 게 있다는 가설은 꽤 세게 밀고 있음.
  - 그리고 더 큰 모델일수록 이런 언어-독립적 회로가 더 잘 보인다는 주장도 있다. 이건 꽤 중요해 보임.

- Entity recognition / hallucination
  - 이 부분도 좋았다.
  - 모델이 "내가 이 이름을 아는가?" 비슷한 internal signal을 갖고 있고, 그게 refusal/uncertainty랑 연결된다는 그림.
  - hallucination을 그냥 factual mistake로 보지 않고, familiarity circuit의 misfire로 본다는 게 좋았음.
  - 약간 "모른다고 말하는 능력"도 회로라는 느낌.

- Refusal / jailbreak
  - 거절도 단순 규칙 lookup이 아니라 꽤 여러 신호가 얽혀 나오는 걸로 나온다.
  - `harmful requests` 같은 추상 feature가 finetuning으로 만들어졌다는 부분도 흥미로웠고.
  - jailbreak 쪽은 더 찝찝했는데, 모델이 처음부터 다 이해하고 저항한다기보다 문맥 흐름에 끌려가면서 반쯤 미끄러지는 느낌이 있다.
  - 보안도 결국 local grammar pressure랑 role pressure를 같이 봐야 하나 싶다.

- Chain-of-thought faithfulness
  - 개인적으로 제일 세게 남은 부분.
  - CoT가 실제 내부 계산과 맞는 경우도 있고,
  - 답을 먼저 정해놓고 거꾸로 이유를 꾸미는 경우도 있고,
  - 사람이 준 힌트에 reasoning이 끌려가는 경우도 있다.
  - 즉 CoT는 로그라기보다 인터페이스일 수 있다.
  - 약간 `working backwards from goal states` 같은 장면을 보는 느낌도 있음.

## 좋았던 문장 / 좋았던 감각

- "black box가 점점 불만족스러워진다"는 문제의식.
- "microscope" 비유.
- "biology" 비유.

이 글이 좋은 건, 괜히 거대한 철학을 말하지 않고 도구의 느낌을 준다는 점이다.
현미경 생겼다. 근데 세포 몇 개만 보인다. 그래도 전보단 낫다.
대충 이런 느낌.

그리고 저자들이 성공 사례 위주라고 인정하는 태도도 좋다.
이거 은근 중요함.
해석가능성 글 읽다 보면 다 본 것처럼 말하는 글이 있는데, 이 글은 오히려 `about a quarter of the prompts` 수준이라고 선을 긋는다.
그 정도면 아직 갈 길 멀다.

> "Like any microscope, our tools are limited in what they can see."

## 아쉬운 점

- attention을 거의 고정해두고 보는 방식이라 "왜 저걸 봤냐"는 설명은 약하다.
- 그래서 fetch 이전보다 fetch 이후 설명이 강함.
- 나는 여기서 약간 걸렸다.

- feature abstraction level이 너무 들쭉날쭉함.
  - 어떤 feature는 너무 좁고
  - 어떤 feature는 사람 해석이 많이 들어간 것 같고
  - 결국 supernode로 수작업 묶는 순간 사람이 꽤 많이 개입한다.

- prompt-specific graph는 재밌는데, global circuit으로 넘어가면 바로 흐려진다는 한계도 솔직히 큼.
- 그래서 이걸로 "모델은 원래 이렇게 작동한다"까지 일반화하면 좀 위험할 것 같다.

- mechanistic faithfulness 문제도 남아 있다.
  - replacement model이 causal하게 같은 계산을 한 건지
  - 아니면 training distribution에서 비슷한 output만 낸 건지
  - 이건 끝까지 찝찝함.

## 적어두고 싶은 것

- 모델이 생각보다 중간 표현을 많이 쓴다.
- 그 중간 표현은 token 표면형보다 더 추상적일 수 있다.
- planning 비슷한 것도 있다.
- refusal도 단순 guardrail 한 줄이 아니다.
- CoT는 믿을 수도 있고 못 믿을 수도 있다.

이 정도만 해도 사실 꽤 큼.

## 한줄 평

- "LLM 안에 회로가 있다"는 말을 제일 설득력 있게 보여준 글 중 하나.
- 다만 이게 전체 설계도는 아니고, 좋은 현미경에 가깝다.
- 그리고 이런 종류의 글은 앞으로 더 길어질 것 같음. 아직 시작 느낌.


> One reason models are difficult to interpret is that their neurons are typically polysemantic – that is, they perform many different functions that are seemingly unrelated. 4 To circumvent this issue, we build a replacement model that approximately reproduces the activations of the original model using more interpretable components. Our replacement model is based on a cross-layer transcoder (CLT) architecture, which is trained to replace the model’s MLP neurons with features, sparsely active “replacement neurons” that often represent interpretable concepts. In this paper, we use a CLT with a total of 30 million features across all layers.