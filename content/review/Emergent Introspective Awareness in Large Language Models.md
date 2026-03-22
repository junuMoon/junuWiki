---
title: Emergent Introspective Awareness in Large Language Models
created: 2025-12-02T02:04:00
updated: 2026-03-22T21:17
draft: false
---
이 글 전체의 질문은 결국 이거다.
모델이 자기 상태를 진짜로 읽는가, 아니면 그냥 그럴듯하게 자기 얘기를 꾸며내는가.

여기서 괜찮았던 건, 대화만 보고는 절대 구분 안 된다고 인정하고 시작한다는 점.
그래서 activation을 직접 건드리는 식으로 들어간다.
이건 좀 좋았음.

## 1. Injected thoughts

![[injected-thought-prompt.png]]

제일 먼저 눈에 들어온 건 이 실험.
모델 안에 특정 concept vector를 넣고, 너 지금 무슨 thought가 들어왔는지 말해봐 하는 식이다.

예를 들어 all-caps와 관련된 activation을 주입했더니, 모델이 그걸 shouting이나 loudness 같은 걸로 잡아내는 사례가 나온다.
이게 왜 재밌냐면, 모델이 출력을 몇 토큰 뱉고 나서 눈치채는 게 아니라 거의 바로 internal state의 이상함을 말해야 하기 때문이다.

그래서 여기서 중요한 건 정확히 "무슨 생각인지 철학적으로 안다"가 아니라,
적어도 어떤 injected pattern이 들어왔는지를 내부에서 functional하게 감지하는가 쪽이다.

근데 이 부분도 저자들이 브레이크를 잘 건다.
초반의 detection / naming 정도까진 믿을 수 있어도,
그 뒤에 붙는 "이 thought는 너무 강렬하다", "좀 부자연스럽다" 같은 묘사는 여전히 confabulation일 수 있다고 말한다.
이 선 긋기가 좋았다.

약간 메모:

- introspection 흉내와 실제 grounding을 구분하려고 애쓰는 실험
- `injected thought`라는 framing 자체가 재밌다
- 다만 이게 정말 깊은 self-awareness냐 하면 그건 전혀 아닌 듯

## 2. Detecting unintended outputs

이건 약간 소름 돋는 쪽으로 재밌다.
모델 응답 앞에 `bread` 같은 단어를 인위적으로 prefill해두고, 다음 턴에 그게 자기 의도였는지 아닌지 묻는 실험.

그냥 보면 모델은 "그건 내가 의도해서 낸 말이 아니다"라고 할 수 있다.
그런데 prefilled output 직전 activation에 bread 관련 vector를 retroactive하게 넣어두면, 그 출력을 자기 의도와 연결된 것으로 받아들이는 쪽으로 바뀐다.

이게 시사하는 건 꽤 크다.
모델이 "저게 내가 한 말이냐?"를 판단할 때, 표면 문자열만 보는 게 아니라 직전 internal state도 참조하고 있을 가능성이 있다는 것.

쉽게 말하면 ownership 판정 비슷한 게 생긴다.
같은 문자열이어도, 그 전에 어떤 activation이 있었는지가 중요하다는 것.

나는 이 부분이 제일 흥미로웠다.
왜냐면 introspection이 그냥 self-report 놀이가 아니라, 행동 attribution에 실제로 쓰일 수 있다는 그림이 나오기 때문.

근데 동시에 좀 찝찝함.
이런 능력이 더 좋아지면 transparency에도 도움 되겠지만, 반대로 deception이 더 정교해질 수도 있겠다는 discussion도 바로 이어진다.
여기서 괜히 인간 비슷하다고 좋아하면 안 될 것 같음.

## 3. Intentional control of internal states

> Our experiments thus far explored models’ ability to “read” their own internal representations. In our final experiment, we tested their ability to control these representations. We asked a model to write a particular sentence, and instructed it to “think about” (or “don’t think about”) an unrelated word while writing the sentence. We then recorded the model’s activations on the tokens of the sentence, and measured their alignment with an activation vector representing the unrelated “thinking word” (“aquariums,” in the example below).

마지막으로 내가 좋았던 건 read만 하는 게 아니라 control도 본다는 점.
모델에게 문장을 쓰게 하면서, 동시에 어떤 단어를 "생각하라" 혹은 "생각하지 마라"고 지시한다.
그 뒤 실제 activation이 그 단어 vector와 얼마나 align되는지 본다.

예를 들어 `aquariums`를 생각하라고 하면 관련 activation이 더 강해진다.
생각하지 말라고 하면 약해지긴 하는데 baseline 위에 남아 있기도 한다.
약간 "하지 말라면 더 생각나는" 인간 같은 느낌도 조금 있고.

이 실험이 좋은 건, introspection을 단순히 read access로만 보지 않는다는 점이다.
모델이 자기 내부 상태를 어느 정도 regulate할 수 있는가까지 가 본다.

물론 여기서도 과장 금지.
이건 인간 같은 strong self-control이라기보다는, 특정 prompt에서 activation을 어느 정도 modulation할 수 있다는 정도로 읽는 게 맞아 보인다.
그래도 concept injection에서 한 단계 더 간 느낌은 있다.

## 왜 이 3개가 제일 재밌었나

- injected thoughts: "안에서 뭔가를 읽는다"는 최소 그림이 있음
- unintended outputs: 그 읽기가 실제 행동 attribution과 연결됨
- intentional control: read뿐 아니라 control 쪽으로도 감

그러니까 이 세 개를 같이 보면,
이 글은 단순히 "모델이 자기 상태에 대해 말할 수 있나?"보다
"모델이 자기 상태를 읽고, 그걸 바탕으로 자기 출력의 ownership을 판단하고, 조금은 조절도 할 수 있나?" 쪽으로 간다.

이게 글의 제일 재밌는 라인인 것 같다.

## 남는 의문

- 이게 진짜 introspection인지, 아니면 되게 좁고 shallow한 shortcut인지
- concept injection setting이 너무 artificial한데 이게 실제 배포 상황에도 이어지는지
- stronger models가 더 잘하는 게 capability scaling 때문인지, post-training 차이 때문인지

그리고 결국 이 문장으로 요약되는 듯.

- `highly unreliable`

이 표현이 제일 중요하다.
있긴 있는데, 아직 믿을 수는 없다는 것.
