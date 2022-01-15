---
title: "react build 에러"
date: 2021-10-23 12:00
categories: algorithm
---

2022년 1월 15일 현재 npx를 사용해서 react 앱을 만들면 몇가지 warning이 나오지만 npm run start를 실행하기에는 문제가 없습니다. 다만 npm run build 명령어를 실행하면 실패하게 되는데요. 

```
"TypeError: MiniCssExtractPlugin is not a constructor" in fresh CRA installation
```

npm을 사용하는 경우에는 다음 명령어로 해결할 수 있습니다.

```
npm i -D --save-exact mini-css-extract-plugin@2.4.5
```
