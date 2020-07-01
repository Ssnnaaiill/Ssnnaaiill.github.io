---
layout: post
current: post
cover: assets/images/bus.jpg
navigation: True
title: GraphQL에서 해결할 수 있는 REST에서의 문제점들
date: 2020-01-02
tags: [API, GraphQL, Backend]
class: post-template
subclass: "post"
author: Ssnnaaiill
---

백엔드 작업을 할 때에는 REST API를 사용했는데, 선임분께서 GraphQL을 배워보면 엄청난 도움일 될 것이라고 조언해 주셔서 [노마드코더 유튜브](https://www.youtube.com/channel/UCUpJs89fSBXNolQGOYKn0YQ)에서 GraphQL로 영화 API를 만드는 영상을 찾아보게 되었습니다.

GraphQL로 REST API에서 발생할 수 있는 문제들을 간편하게 처리할 수 있다고 합니다.

## Over-fetching

API에서 특정 데이터를 불러오고 싶을 때,

```js
axios.get("/api").then(res => { ... });
```

다음과 같이 `GET` 요청을 하기 마련입니다.

하지만 api에서 우리가 원하는 데이터만 골라서 제공하지 않고 다른 정보들까지 포함해서 response하는 경우도 있습니다. 사용하지 않을 영역까지 요청하게 된다는 것이지요. 효율적이지 못하고 어플리케이션이 무거워지게 만드는 요인 중 하나입니다.

이렇게 서버에서 불필요한 정보들까지 요청받는 것을 `Over-fetching`이라고 합니다.

## Under-fetching

`Under-fetching`은 하나의 어플리케이션, 또는 페이지를 완성하기 위해 REST 서버에 여러 번 요청을 하는 것이라고 합니다.

처음에는

- 여러 번 요청하는 게 어때서?
- 속도가 느려지는 건 알지만, 원하는 기능을 구현하기 위해서는 어쩔 수 없는 과정이잖아?

라고 생각했는데요, GraphQL을 사용하면 Over-fetching, Under-fetching을 겪을 필요 없이 한 번의 요청으로 개발자가 원하는 정보만을 쏙쏙 골라서 response받을 수 있다고 합니다.

## GraphQL에서 필요한 정보만 골라오기

GraphQL에서는 URL 체계 자체가 없습니다. 이 때문에 REST와 커다란 차이가 생깁니다. 그러면 어떻게 서버에서 데이터를 요청해서 가져올 수 있는 걸까요?

### 단 하나의 End Point

GraphQL에서는 End Point가 딱 하나밖에 없습니다. 이 하나뿐인 End Point에서 모든 정보를 가져올 수 있다고 합니다. 모든 정보를 하나의 *쿼리*로 만들 수 있지요. 이 쿼리를 GraphQL 백엔드에 보내면 다음과 같은 요청 정보를 담은 자바스크립트 Object를 보낼 것입니다.

여기서 *쿼리*는 다음과 같은 형태를 띕니다.

```graphql
query {
  feed {
    comments
    likes
  }
  notifications {
    isRead
  }
  user {
    username
    profilePic
  }
}
```

쿼리는 GraphQL 언어로 구성되어 있지요.
그리고 백엔드에서 response한 Object는 다음과 같은 형태를 띕니다.

```json
{
  "feed": [
    {
      "comments": 3,
      "likes": 10
    }
  ],
  "notifications": [
    {
      "isRead": true
    },
    {
      "isRead": false
    }
  ],
  "user": {
    "username": "Yeonseo Jung",
    "profilePic": "snail.png"
  }
}
```

이런 식으로 JSON 형태로 개발자가 요청한 정보들만을 보내준다고 합니다.
