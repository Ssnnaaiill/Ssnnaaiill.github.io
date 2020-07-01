---
layout: post
current: post
cover: assets/images/safari-og.jpg
navigation: True
title: Safari 13.1에서 달라진 것들
date: 2020-05-13
tags: [Github]
class: post-template
subclass: "post"
author: Yeonseo Jung
---

지난 4월, 애플에서 Safari 브라우저의 [최신 버전](https://webkit.org/blog/10247/new-webkit-features-in-safari-13-1/)인 Safari 13.1 (macOS, Catalina, iPadOS, iOS, watchOS)을 릴리스했습니다.

애플의 플랫폼들에게 있어 다양한 개선사항이 부여되었는데, 이번에 발표된 최신 버전의 사파리는 사용자의 개인 정보 보호, 브라우저 성능 최적화 및 브라우저 사용 시 개발자 경험(Dx) 개선에 중점을 두었다고 합니다.

> [New Webkit features in Safari 13.1](https://webkit.org/blog/10247/new-webkit-features-in-safari-13-1/)

주요 업데이트 사항은 다음과 같습니다.

- [Web Animations API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API) 지원
- [비동기 Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [ResizeObserver API](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) 추가
- [CSS Shadow Parts](https://www.w3.org/TR/css-shadow-parts-1/) 지원
- [지능형 추적 방지](https://webkit.org/blog/7675/intelligent-tracking-prevention/)
- 커스텀 AR 훑어보기
- 리디자인 된 컬러피커
- 웹 검사기 업데이트
- 성능 개선

## 지능형 추적 방지

지능형 추적 방지(Intelligent Tracking Prevention)은 2017년 출시된 웹킷의 기능 중 하나로, 서드파티 쿠키를 차단하고 웸사이트가 데스크탑과 모바일 디바이스 모두에서 사용자 데이터 수집을 막음으로써 크로스 사이트 추적을 줄이는 것에 중점을 두었습니다.

이번 업데이트로 인해 7일간의 사파리 사용 후 서드파티 쿠키 차단 및 쿠키 이외의 웹사이트 데이터에 대한 만료를 포함하여, 지능형 추적 방지에 대한 몇 가지 새로운 개선 사항이 제공됩니다. 애플의 엔지니어인 [John Wilander](https://twitter.com/johnwilander)의 [포스팅](https://webkit.org/blog/10218/full-third-party-cookie-blocking-and-more/)에서 자세한 사항을 확인해 볼 수 있습니다.

### Full 서드파티 쿠키 차단

![](https://www.adexchanger.com/wp-content/uploads/2019/12/cookieblocking-scaled.jpg)

이제는 크로스 사이트 리소스에 대한 쿠키가 기본적으로 전면 차단됩니다. 이로써 사용자 정보 보호 시스템이 한층 향상되어 사용자의 동작을 추적하기 훨씬 어려워졌습니다.

쿠키 차단을 통해 감지할 수 있는 ITP 상태가 있는지, 없는지 확인하여, 쿠키 차단의 상태 저장을 제거하고, `로그인 지문`을 비활성화합니다. 그러므로 웹사이트에서 사용자가 로그인된 플랫폼을 없게 되지요.

> [로그인 지문을 테스트해볼 수 있는 라이브 데모](https://robinlinus.github.io/socialmedia-leak/)

### 7일간 모든 브라우저 저장 옵션 사용 가능

기존의 클라이언트 측 쿠키 제한과 함께 ITP는 웹사이트에서 사용자의 상호작용 여부와 관계 없이 7일의 기한이 지난 이후부터 웹사이트의 스크립트 작성 가능 저장소를 모두 삭제하기 시작할 것입니다. 여기에는 다음과 같은 형태의 저장소들이 포함됩니다.

- Indexed DB
- LocalStorage
- Media Keys
- SessionStorage
- Service Worker registrations and cache

사용자가 웹사이트를 방문할 때마다 local storage와 같은 저장소에 방문 정보가 저장됩니다. 이번 업데이트를 통해, 사용자가 전에 방문했던 웹사이트를 7일 동안 다시 방문하지 않고 사파리를 통해 다른 웹사이트만 접속한다면, 해당 웹사이트에 관한 저장된 데이터가 삭제됩니다. 사용자가 사파리 브라우저를 아예 사용하지 않거나, 7일 이내에 그 웹사이트에 다시 접속한다면 삭제되지 않는다고 합니다.

## 커스텀 AR 훑어보기

![](https://imageog.flaticon.com/icons/png/512/64/64943.png?size=1200x630f&pad=10,10,10,10&ext=png&bg=FFFFFFFF)

컨텐츠 제작자들은 웹 배너가 AR view를 오버레이하도록 커스텀할 수 있고, 사용자들은 여기서 AR 경험을 시작할 수 있습니다. 커스텀할 수 있는 항목들은 다음과 같습니다.

- 애플 플레이 버튼 스타일
- 액션 버튼 라벨
- 아이템 제목
- 아이템 부제목
- 가격

![](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/05/applepaybuttons.png?w=531&ssl=1)

또는, HTML로 아예 커스텀 배너를 생성할 수도 있습니다.

```html
https://example.com/example.usdz#custom=https://example.com/customBanner.html
```

AR 훑어보기에 대한 더 많은 정보는 [이 문서](https://developer.apple.com/documentation/arkit/adding_an_apple_pay_button_or_a_custom_action_in_ar_quick_look)에 잘 정리되어 있습니다.

## JavaScript 추가사항

이번 업데이트로 사파리에서 자바스크립트 `replaceAll()` 메소드를 지원하게 되었습니다. (드디어!) ES2020에서 사용 가능한 nullish(`??`) 오퍼레이터 또한 지원한다고 하네요.

```js
"So sorry but I love you".replaceAll(" ", "-");

// So-sorry-but-I-love-you
```

MDN docs에 의하면

## CSS 추가사항

몇 가지 폰트 키워드들이 사용 가능하게 되었습니다. `ui-serif`, `ui-sans-serif`, `ui-monospace`, `ui-rounded` 같은 것들이요.

또한 각 문자마다 (문장 부호나 단어 사이 공백 포함) soft wrap을 추가하고 줄 바꿈에 대한 제한을 무시하게 하는 `line-break: anywhere;` value 또한 사용할 수 있게 되었습니다. `line-break`에 대한 것은 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/CSS/line-break)를 참조하면 더 도움이 될 것 같네요.

`@media` 쿼리에서 사용할 수 있는 기능도 하나 늘어났습니다. `dynamic-range`는 개발자가 표시 기능에 특정한 스타일을 만들 수 있도록 합니다.

```css
@media (dynamic-range: standard) {
  .example {
    /* Styles for displays not capable of HDR. */
    color: rgb(255, 0, 0);
  }
}

@media (dynamic-range: high) {
  .example {
    /* Styles for displays capable of HDR. */
    color: color(display-p3 1 0 0);
  }
}
```
