# Craco를 사용하여 절대경로 설정하기

프로젝트를 진행하다보면 폴더 구조가 점점 복잡해지게된다. 

import시 상대경로를 사용하면 `'../../src/one/tow/three'`  와 같아 한눈에 파악하기 힘들고, 헷갈릴 수 있다.

Create-React-App에서 절대경로를 설정해 주려면 eject를 실행해 webpack을 커스텀해줘야 하는데 숨겨진 설정들이 모두 꺼내지기 때문에 구조상으로 매우 복잡해지고, 한번 실행하면 이전의 상태로 되돌릴 수 없다고 한다.

사내 프로젝트 진행시에 [Craco](https://github.com/gsoft-inc/craco)를 자주 활용했는데 설정 방법을 정리해보려한다.

## 1. Craco란?

[https://github.com/gsoft-inc/craco](https://github.com/gsoft-inc/craco)

root에 craco.config.js 파일을 추가함으로 eject을 실행하지 않고도, 간편히 eslint, babel, postcss등을 재구성할 수 있다.

## 2. Craco 설치 및 설치

craco를 설치한다.

```
yarn add @craco/craco
yarn add craco-alias

or

npm install @craco/craco
npm install craco-alias
```

프로젝트 root에 craco.config.js 파일을 생성하고, 설정을 작성한다.

{% code title="// craco.config.js" %}
```bash
const CracoAlias = require("craco-alias");

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: "options",
        baseUrl: "./src",
        aliases: {
          "@components": "/components",
          "@features": "/features",
          "@assets": "/assets",
          "@data": "/data",
          "@api": "/api",
          "@app": "/app",
        },
      },
    },
  ],
};

```
{% endcode %}

아래 처럼 절대경로 사용이 가능하다.

```bash
import { Button, Input } from "@components";
import { SignUp } from "@features";
```

## 3.  끝 😬

