# 8장, Node.js 타입 분석하기

- Node.js가 기본으로 제공하는 내장 모듈을 활용한 코드에 대한 타입
- Node.js는 기본적으로 CommonJS 모듈시스템
  - import fs = require(’fs’)
  - but, tsconfig.json에서 esModuleInterop 옵션이 활성화 ⇒ import fs from ‘fs’ 가능

### @types/node 패키지 분석

- package.json 파일을 살펴본 결과, **진입점 파일은 index.d.ts**👀

```jsx
[package.json]

// NOTE: These definitions support NodeJS and TypeScript 4.9+.

// Reference required types from the default lib:
/// <reference lib="es2020" />
/// <reference lib="esnext.asynciterable" />
/// <reference lib="esnext.intl" />
/// <reference lib="esnext.bigint" />

// Base definitions for all NodeJS modules that are not specific to any version of TypeScript:
/// <reference path="assert.d.ts" />
/// <reference path="assert/strict.d.ts" />
/// <reference path="globals.d.ts" />
/// <reference path="async_hooks.d.ts" />
/// <reference path="buffer.d.ts" />
...

/// <reference path="globals.global.d.ts" />
```

- /// <reference lib /> 은 어떤 lib 파일을 기본적으로 포함할 지를 적어둔 것
  - es2020,esnext.asynciterable,esnext.intl,esnext.bigint 등등..
- /// <reference path />
  - 각각의 .d.ts 파일은 Node.js 모듈과 대응
  - 이 중에서 globals.d.ts 와 globals.global.d.ts는 전역 객체에 대한 타입 담당

### ⚪ declare module ‘모듈명’ 과 declare module ‘node: 모듈명’

- declare module ⇒ 타입스크립트에 해당 모듈이 있다는 것을 알림
  - 해당 모듈에 대한 타입 선언도 이 블록 안에 있음을 알림
- declare module ‘node:모듈명’ ⇒ Node.js에서 권장하는 내장 모듈 import 방법

```jsx
declare module 'path' {
	...
	const path: path.PlatformPath;
	export = path;
}
declare module 'node:path'{
	import path = require('path')
	export = path;
}
```

- export \* from ‘http’는 http 모듈의 모든 것을 import 후, node:http모듈에서 export 하는 것을 의미
  - node:http 모듈과 http 모듈은 동일한 것을 export 하게 됨
  - tthp 모듈을 import 하든, node:http 모듈을 import 하든 동일하게 사용 가능
