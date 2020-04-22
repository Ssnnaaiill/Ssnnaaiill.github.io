---
layout: post
title: Vuetify에서 Jest 사용 시 초기 설정 셋업
subtitle:
tags: [Jest, Vue, Frontend, Javascript]
author: Yeonseo Jung
comments: False
---

# 오류

[Vuetify](https://vuetifyjs.com/)는 Vue 프로젝트 개발 시 자주 사용되는 UI 라이브러리입니다. Vue CLI로 Vuetify를 프로젝트에 추가한 후 Jest로 유닛 테스트를 하려 하면 아래와 같은 오류가 콘솔에 표시된다.

```js
console.error node_modules/vue/dist.vue.runtime.common.js:602 [Vue warn]: Unknown custom element: <v-container> - did you register the component correctly? For recursive components, make sure to provide the "name" option.

found in

----> <Anonymous>
        <Root>

// ...
```

Jest에서 Vuetify를 불러올 수 없다고 합니다. 이 오류는 Vuetify 라이브러리의 문제로, 2018년에 해당 문제에 관한 [issue](https://github.com/vuetifyjs/vuetify/issues/4964)가 올라왔으나 2년이 다 되어가는 현재까지 아직 해결되지 않은 Future Request 상태입니다.

## 해결책

`/src/test/jest-setup.js` 파일을 생성하여 unit test 시 Vue와 Vuetify 초기 설정을 할 수 있도록 합니다.

```js
import Vue from "vue";
import Vuetify from "vuetify";

Vue.use(Vuetify);
```

또한 테스트를 수행할 때마다 `jest-setup.js` 파일이 실행될 수 있도록 `jest.config.js` 파일이나 Webpack의 jest property에 기입하도록 합니다.

```js
// jest.config.js

module.exports = {
  // ...
  setupFilesAfterEnv: ["<rootDir/>/tests/jest-setup.js"],
  // ...
};
```

이렇게 설정해 준 후 다시 테스팅하면 앞에서 본 오류 메시지는 더 이상 보이지 않았습니다.

하지만 가끔 다음과 같은 또 다른 오류가 발생할 때가 있다고 합니다.

```bash
[Vuetify] Unable to locate target [data-app] in Component
```

Vuetify의 초기화 방식 때문에 일어나는 현상입니다. Vuetify는 초기화 시 `<v-app></v-app>` 태그로 Vuetify 컴포넌트들을 감쌉니다. `data-app`은 이 태그의 attribute인데, Jest unit test를 할 때에는 컴포넌트 단위로 테스트합니다. 그러다보니 `<v-app></v-app>` 태그를 빼고 선언하게 되어서 위와 같은 오류가 발생하는 것입니다.

이 경우 `jest-config.js` 파일에 `data-app` attribute를 추가합니다.

```js
document.body.setAttribute("data-app", true);
```
