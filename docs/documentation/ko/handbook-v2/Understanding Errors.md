---
title: Understanding Errors
layout: docs
permalink: /ko/docs/handbook/2/understanding-errors.html
oneline: "TypeScript에서 오류를 보는 방법."
---

# 오류를 이해하기 (Understanding Errors)

TypeScript는 오류를 찾으면, 무엇이 잘못됐는지 가능한 자세하게 설명하려고 합니다.
TypeScript의 타입 시스템은 구조적이기 때문에, 문제를 발견한 위치에 대해 자세한 설명을 제공할 수 있습니다.

## 용어 (Terminology)

오류 메시지에서 자주 등장하는 용어를 알면 이해하는 데 도움이 됩니다.

#### *할당할 수 있는* (*assignable to*)

TypeScript는 타입이 다른 타입으로 대체할 수 있을 때 타입을 다른 타입에 *할당할 수* 있다 라고 표현합니다.
다시 말해 `고양이`는 `동물`을 대체할 수 있기 때문에 `동물`에게 *할당할 수* 있습니다.

이름에서 보이듯, 이런 관계는 `t`와 `s`의 타입을 검사하여 `t = s;`의 할당 타당성을 확인하는 데 사용됩니다.
또한 두 가지 타입이 상호 작용하는 대부분의 위치에서 확인할 때에도 사용됩니다.
예를 들어, 함수를 호출할 때 각 인수의 타입은 매개 변수로 선언된 유형에 *할당할 수* 있어야 합니다.

비공식적으로 `T is not assignable to S`라고 하면 TypeScript는 "*`T`와 `S`는 호환되지 않는다"*.고 말한다고 생각하면됩니다.
그러나, 이것은 *방향성이 있는* 관계라는 점에 유의하세요: `S`가 `T`에 할당될 수 있다고 해서 `T`가 `S`에 할당될 수 있는 것은 아닙니다.

## 예시들 (Examples)

오류 메시지의 몇 가지 예제를 살펴보고 어떤 일이 일어나고 있는지 알아보겠습니다.

### 오류 정교화 (Error Elaborations)

각 오류는 선행 메시지로 시작하고 때로는 더 많은 하위 메시지로 이어집니다.
각 하위 메시지는 위의 메시지에 대한 "왜?" 질문에 대답하는 것으로 생각할 수 있습니다.
몇 가지 예를 통해 실제로 어떻게 작동하는지 살펴보겠습니다.

다음은 예제 자체보다 긴 오류 메시지를 생성하는 예입니다:

```ts twoslash
// @errors: 2322
let a: { m: number[] }
let b = { m: [""] }
a = b
```

마지막 줄에서 TypeScript는 오류를 발견했습니다.
오류 발생에 대한 논리는 할당이 정상인지 확인하는 논리에서 비롯됩니다:

1. `b` 타입은 `a` 타입에 할당 가능한가요? 아뇨. 왜요?
2. 왜냐하면 `m` 속성의 타입이 호환되지 않기 때문입니다. 왜죠?
3. 왜냐하면 `b`의 `m` 속성(`string[]`)은 `a`의 `m` 속성(`number[]`)에 할당할 수 없기 때문입니다. 왜죠?
4. 한 배열의 요소 타입(`string`)을 다른 타입(`number`)에 할당할 수 없기 때문입니다.

### 추가 속성 (Extra Properties)

```ts twoslash
// @errors: 2322
type A = { m: number }
const a: A = { m: 10, n: "" }
```

### 유니언 할당 (Union Assignments)

```ts twoslash
// @errors: 2322
type Thing = "none" | { name: string }

const a: Thing = { name: 0 }
```
