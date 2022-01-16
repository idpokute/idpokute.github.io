---
title: "태그의 타입을 리액트 타입스크립트에 인식못할 때"
date: 2021-10-23 12:00
categories: typescript
---

vscode에서 리액트와 타입스크립트를 쓰고 있는데 모든 태그에 빨간 밑줄과 함께 다음의 경고가 뜹니다.

```
 [ts] JSX element implicitly has type 'any' because no interface 'JSX.IntrinsicElements' exists.
```

이를 해결하려면 node_modules 폴더를 지우고 다시 `npm install`을 실행하면 다시 인식을 하게 됩니다.
