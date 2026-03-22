---
title: 타입(Type)과 스키마(Schema)
created: 2026-03-22T17:27
updated: 2026-03-22T17:54
---
- 타입(Type): 코드 안에서 값의 모양을 설명하는 정적 약속
- 스키마(Schema): 실제 데이터가 그 약속을 지키는지 검사하는 런타임 규칙

## 타입

타입은 값의 형태와 성질을 표현하는 정적 규칙이다. 함수의 인자와 반환값, 객체의 필드 구조, 변수에 들어갈 수 있는 값의 범위를 정의한다. 타입은 컴파일 시점에 코드의 일관성과 안전성을 검사하는 기준이며, 보통 런타임에는 유지되지 않는다.

```ts
type User = {
  name: string
  age: number
}

const user: User = {
  name: "junu",
  age: 20,
}
```

## 스키마

스키마는 실제 데이터 검증 규칙이다. API 응답, 사용자 입력, JSON 파일처럼 외부에서 들어오는 값이 정말 우리가 기대한 모양인지 확인할 때 쓴다. TypeScript에서는 `zod` 같은 라이브러리로 스키마를 자주 만든다. 스키마는 실행 중에도 동작하므로 잘못된 데이터를 걸러낼 수 있다.

```ts
import { z } from "zod"

const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
})

const result = UserSchema.safeParse({
  name: "junu",
  age: "20",
})

console.log(result.success) // false
```

## 언제 타입을 쓰고 언제 스키마를 쓰는가

### 타입

- 함수 인자와 반환값을 명확히 하고 싶을 때
- 객체 구조를 코드 수준에서 설명하고 싶을 때
- 자동완성과 정적 분석의 도움을 받고 싶을 때
- [[제네릭 타입(Generic Type)]]처럼 입력 타입에 따라 결과 타입이 달라지도록 표현하고 싶을 때

### 스키마

- API 요청과 응답을 검증할 때
- 사용자 입력을 검사할 때
- `localStorage`, 파일, JSON 같은 외부 데이터를 읽을 때
- 신뢰할 수 없는 데이터를 안전하게 처리해야 할 때
