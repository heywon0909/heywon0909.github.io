# 9장, Express 타입 분석하기

### 진입점 파일 확인하기 = index.d.ts

```jsx
// Type definitions for Express 4.17
// Project: http://expressjs.com
// Definitions by: Boris Yankov <https://github.com/borisyankov>
//                 China Medical University Hospital <https://github.com/CMUH>
//                 Puneet Arora <https://github.com/puneetar>
//                 Dylan Frankland <https://github.com/dfrankland>
// Definitions: https://github.com/DefinitelyTyped/DefinitelyTyped

/* =================== USAGE ===================

    import express = require("express");
    var app = express();

 =============================================== */

/// <reference types="express-serve-static-core" />
/// <reference types="serve-static" />

import * as bodyParser from 'body-parser';
import * as serveStatic from 'serve-static';
import * as core from 'express-serve-static-core';
import * as qs from 'qs';

declare function e(): core.Express;
declare namespace e {

}
export = e;
```

- Express 4.17 버전임
- express-server-static-core와 serve-static 패키지의 타입을 참고
- body-parser,serve-static,express-server-static-core,qs패키지 타입을 import
- **e는 함수이자 네임스페이스를 export**

[⚪ 스크립트 파일과 모듈 파일 이해하기](https://www.notion.so/abac12f6cf8344939c8ccc6e8d015ec2?pvs=21)
