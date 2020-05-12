---
layout: post
title: TypeScipt vs Flow
subtitle: Javascript 프로젝트에서 사용할 type extension 고르기
tags: [flow, typescript]
author: Yeonseo Jung
comments: False
---

## 개요

![](https://miro.medium.com/max/3000/1*VAP896WLOAJNH8uspXsbTA.png)

자바스크립트는 동적 타입 언어(Dynamically Typed Language)입니다. 사용자가 할당해주는 값에 따라 그 변수의 데이터 타입이 바뀌지요. 타입 제한은 딱히 없습니다. Type annotation을 해주는 내장 방식도 없습니다.

그런 것을 구현하기 위해서는 우선 다른 언어로 프로그램을 작성한 후 다시 자바스크립트로 변환시켜주는 작업을 거쳐야 합니다. 이런 작업이 꽤나 번거롭기 때문에, 자바스크립트의 변수 타입을 체크해주고 제한해주는 프로그래밍 언어(Typed Extension)가 필요합니다.

대표적으로 사용되는 두 가지는 [마이크로소프트의 TypeScript](https://www.typescriptlang.org/docs/home.html)와 [페이스북의 Flow](https://flow.org/)가 있지요. 이 둘의 기능을 살펴보고 비교해 보도록 합시다.

|           | TypeScript  | Flow       |
| --------- | ----------- | ---------- |
| 개발사    | Microsoft   | Facebook   |
| 최초 공개 | 2012.10.01  | 2014.11.18 |
| 라이센스  | Open srouce | Facebook   |
| 커뮤니티  | 대규모      | 소규모     |

## 개발 경험

Dx는 다양한 라이브러리와 프레임워크, 사용하기 쉬운 정도, 그리고 전반적인 생산성 등으로 측정될 수 있습니다.

- [Typescript Document](https://www.typescriptlang.org/docs/handbook/basic-types.html)
- [Flow Document](https://flow.org/en/docs/)

## TypeScript

지원에 관한 측면에서 보자면, TypeScript가 훨씬 우세합니다. React, Vue, Angular등과 같은 인기 있는 프론트엔드 프레임워크들이 TypeScript를 지원해주고 있기 때문이지요.

![](https://i.imgur.com/XOg8WPl.png)

> CRA를 통해 React 프로젝트를 생성할 때 `--template tyescript` 옵션을 적용하면 프로젝트에 TypeScript가 적용됩니다.

![](https://i.imgur.com/RUyu5aE.png)

> Vue CLI의 manual option 선택을 통해 TypeScript가 적용된 Vue 프로젝트를 생성할 수 있습니다.

TypeScript를 사용하게 된다면 다양한 브라우저를 지원할 수 있도록 자동으로 JavaScript(주로 ES5)로 변환할 수 있도록 구성할 수 있습니다.

Vue에서의 예를 들어보자면, 컴포넌트를 생성할 때 Vue를 참조하게 되면 TypeScript의 타입 유추가 나타나게 됩니다.

```js
import Vue from "vue";

const Component = Vue.extend({
  // ...
});
```

또한 TypeScript로 컴포넌트를 작성할 수도 있습니다. Vue에서는 다음과 같이 class를 달아주는 방식을 사용합니다.

```vue
<template>
  <button @click="onClick">Click!</button>
</template>

<script lang="ts">
import * as Vue from "vue";
import { Component } from "vue-property-decorator";

@Component()
class App extends Vue {
  public message: string = "Hello World";

  public onClick(): void {
    window.alert(this.message);
  }
}
</script>
```

TypeScript 어노테이션을 보면 어쩐지 눈에 익은 자바스크립트 syntax들이 보이는 것 같기도 합니다.

Angular의 경우, 아예 프레임워크 자체가 TypeScript로 개발되었고 Angular 프로젝트에서 TypeScript를 사용해서 개발하는 것을 권장하고 있습니다. Plain JavaScript로 개발할 수 있는 옵션이 있기는 하지만 거의 사용되지 않습니다.

## Flow

한편, Flow는 React에서만 지원되고 있습니다. 아쉽게도 Vue나 Angular에서의 사용은 공식적으로 지원되고 있지 않습니다. 사용하는 [Flow 공식 문서](https://flow.org/en/docs/react/)에 따르면 React 프로젝트에 Flow를 적용하기 위해서는 Babel 설정에 Flow support를 추가한 다음,

![](https://i.imgur.com/iVbPYb6.png)

`.babelrc` 파일에 다음과 같이 프리셋을 추가하면 된다고 합니다.

```js
{
  "presets": ["@babel/preset-flow"]
}
```

TypeScript와 Flow의 syntax 차이는 그렇게 큰 구분점이 되지 않습니다. 양쪽 모두 타입을 비슷하게 지원하고, 타입 어노테이션도 비슷하지요.

```js
function concat(a: string, b: string) {
  return a + b;
}
```

위 코드에 있는 함수는 두 개의 string이 주어지면, 그 둘을 이은 새로운 문자열을 리턴합니다. (매개변수에 대한 타입은 string으로 지정되어 있는 것을 알 수 있습니다.) TypeScript에는 `nullable` 타입이 있고, Flow에는 `maybe` 타입이 있다는 점도 특징입니다. TypeScript, Flow 둘 모두 generic을 가지고 있고, 이 덕분에 변수 타입을 취하는 코드를 작성할 수 있습니다.

예를 들어 다음 함수는 전달받은 값을 그대로 반환합니다.

```js
function identity<T>(value: T): T {
  return value;
}

// we can use it by writing...
indentity < string > "foo";
```

Flow는 TypeScript와 마찬가지로 typecasting을 제공합니다. 작동 방식은 TypeScript와 비슷한데, 콜론(:) 기호를 사용하여 원하는 타입으로 cast할 수 있습니다.

```js
let value = 10;
(value: number);
```

같은 코드를 TypeScript에서는 이렇게 작성합니다.

```js
let value = 10 as number;
```

## Interface

Flow와 TypeScript에는 또 다른 중요한 공통적인 기능이 있는데, 바로 interface입니다. Interface는 object의 구조를 제한하는 타입으로 사용됩니다.

```js
interface PersonInterface {
  id: number;
  firstName: string;
  lastName: string;
  fullName(firstName: string, lastName: string): string;
}
```

이 코드는 Flow와 TypeScript 모두에 적용할 수 있는 interface를 작성한 것입니다. `implements` 키워드로 우리가 작성한 인터페이스를 구현하는 객체나 클래스를 만들 수 있습니다.

```js
interface PersonInterface {
  id: number;
  firstName: string;
  lastName: string;
  fullName(firstName: string, lastName: string): string;
}

class Person implements PersonInterface {
  id: number;
  firstName: string;
  lastName: string;
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  fullName(firstName: string, lastName: string) {
    return `${firstName} ${lastName}`;
  }
}
```

인터페이스에 명시된 모든 요소들을 implement하지 않으면 에러가 나니 주의해야 합니다.

Flow의 한 가지 특이한 점은 주석에 타입 체킹 코드를 넣을 수 있다는 것입니다.

```js
function greet(greeting /*: string*/) /* : string */ {
  return greeting;
}
```

TypeScript에서는 없는 Flow만의 고유한 기능이긴 하지만, 자동완성이나 구문 강조 표시는 지원하고 있지 않으므로 그렇게 큰 이점은 아니라고 볼 수도 있습니다.

## Text Editor 지원과 LSP

Visual Studio Code, WebStorm등과 같은 텍스트 에디터들은 대부분 TypeScript를 built-in으로 지원하고 있습니다. [JetBrain 공식 문서](https://www.jetbrains.com/help/webstorm/typescript-support.html)에서 볼 수 있듯이, WebStorm은 TypeScript 코드 디버깅과 에러 체킹을 내장 지원하고 있습니다. Syntax와 컴파일 에러를 IDE에서 바로 확인할 수 있도록 해주고 있지요. TypeScript를 지원한다는 것은 개발자가 코드를 빌드하기 전에도 에디터 화면에서 전체 타입 검사, 자동완성, 컴파일 에러를 바로 확인할 수 있다는 것을 의미합니다.

![](https://thumbs.gfycat.com/ThreadbareDelightfulFoxhound-size_restricted.gif)

> Lumpy Space Princess와는 무관합니다🔮

Flow에는 없는 TypeScript만의 또 다른 특징은 **LSP(Language Server Protocol)** 가 있다는 것입니다. Flow에서는 로컬에서 타입 체킹을 하기 때문에 Language Server나 type definition이 없습니다. 그러기 위해서는 Flow 컴파일러를 실행해야 합니다. 그렇지만 TypeScript는 그럴 필요가 없습니다. Language Server에서 코드를 작성하는 족족 타입 체킹을 돌려주기 때문이죠.

타입 정의는 Lodash나 moment.js 같은 라이브러리와 함께 제공되기도 합니다. 그 덕에 VS Code와 같은 텍스트 에디터에서 자동 완성이나 타입 검사를 지원받을 수 있지요.

## Developer experience comparison

|                          | TypeScript                                        | Flow                                              |
| ------------------------ | ------------------------------------------------- | ------------------------------------------------- |
| 에디터 및 IDE 지원       | 많음                                              | 없음 ~ 적음                                       |
| Stack Overflow 질문 수   | 100000+                                           | 600+                                              |
| 프레임워크 지원          | React, Vue, Angular 등 다수                       | React only                                        |
| 라이브러리 지원          | 많음                                              | 없음 ~ 적음                                       |
| 자동완성                 | IDE나 에디터에서 지원                             | 없음                                              |
| 컴파일 에러 감지         | IDE나 에디터에서 지원                             | 없음                                              |
| Syntax                   | 포괄적인 타입 체킹, 정적/동적 타입 주석 모두 포함 | 포괄적인 타입 체킹, 정적/동적 타입 주석 모두 포함 |
| Generics                 | 지원함                                            | 지원함                                            |
| 기존 프로젝트에서의 지원 | TypeScript 지원을 위해 TypeScript 패키지 추가     | Babel을 통한 support 추가                         |

## 효율

![](https://static.turbosquid.com/Preview/001335/138/P3/DHQ.jpg)

TypeScript와 Flow의 성능 차이는 미미합니다. TypeScript의 경우 지속적으로 500 ~ 600MB의 RAM을 사용하는데, Flow 또한 그 정도 범위에서 크게 벗어나지 않습니다. 리소스 사용의 효율성 측면에서는 큰 차이가 없습니다. 하지만 TypeScript가 조금 더 빠르고 버그가 덜 난다고 하는 의견도 있다고 합니다. (이 [Reddit 스레드](https://www.reddit.com/r/javascript/comments/7bfwpl/flow_vs_typescript/)를 참고하세요!)

## 커뮤니티 지원과 리소스 및 공식문서 퀄리티

TypeScript의 커뮤니티 지원은 Flow보다 훨씬 발달되어 있다는 것은 앞에서 언급한 바 있습니다. 애초에 React 이외의 Flow를 지원하는 라이브러리는 드뭅니다. Stack Overflow등과 같은 포럼에서도 관련 포스팅은 적고, 공식 도큐먼트도 TypeScript보다 빈약한 편입니다. 반면 TypeScript는 React, Vue, Angular와 같은 프론트엔드 프레임워크는 물론, Express나 Nest.js와 같은 백엔드에서도 사용할 수 있습니다. 범용성에 있어서는 Flow와 천지차이이지요.

더 많은 TypeScript type definition을 보고 싶다면 [여기](http://definitelytyped.org/)에서 확인하실 수 있습니다.

## 결론

저는 개인적으로 TypeScript를 더 애용합니다. 많은 프레임워크에서 지원하기도 하고, 기능 면으로도 마음에 들지만 아무래도 방대한 리소스와 많은 이용자들의 조언이 있었기 때문에 TypeScript를 좀 더 쉽게 접할 수 있었던 것 같아요.

Flow도 Flow만의 매력이 있는 근사한 익스텐션이긴 하지만, 아직까지 사용하는 사람들이 그렇게 많지는 않은 것 같아서 아쉽습니다. 페이스북에서 Flow에 좀 더 관심을 기여하고 지속적으로 개발해 나간다면, TypeScript가 과거의 탈을 벗고 현재 많은 개발자들에게 사랑받게 된 것처럼 점차 Flow 관련 리소스도 늘어나게 될 것 같습니다.
