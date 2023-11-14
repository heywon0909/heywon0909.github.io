# 5장, jQuery 타입 분석하기

### 🔴 entry(진입점)파일 분석하기

```jsx
// Type definitions for jquery 3.5
// Project: https://jquery.com
// Definitions by: Leonard Thieu <https://github.com/leonard-thieu>
//                 Boris Yankov <https://github.com/borisyankov>
//                 Christian Hoffmeister <https://github.com/choffmeister>
//                 Steve Fenton <https://github.com/Steve-Fenton>
//                 Diullei Gomes <https://github.com/Diullei>
//                 Tass Iliopoulos <https://github.com/tasoili>
//                 Sean Hill <https://github.com/seanski>
//                 Guus Goossens <https://github.com/Guuz>
//                 Kelly Summerlin <https://github.com/ksummerlin>
//                 Basarat Ali Syed <https://github.com/basarat>
//                 Nicholas Wolverson <https://github.com/nwolverson>
//                 Derek Cicerone <https://github.com/derekcicerone>
//                 Andrew Gaspar <https://github.com/AndrewGaspar>
//                 Seikichi Kondo <https://github.com/seikichi>
//                 Benjamin Jackman <https://github.com/benjaminjackman>
//                 Josh Strobl <https://github.com/JoshStrobl>
//                 John Reilly <https://github.com/johnnyreilly>
//                 Dick van den Brink <https://github.com/DickvdBrink>
//                 Thomas Schulz <https://github.com/King2500>
//                 Terry Mun <https://github.com/terrymun>
//                 Martin Badin <https://github.com/martin-badin>
//                 Chris Frewin <https://github.com/princefishthrower>
// Definitions: https://github.com/DefinitelyTyped/DefinitelyTyped
// TypeScript Version: 2.7

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

- ///<reference types/>, ///<reference path/> 두 가지 주석
  - ///<reference types/>
    - 지금 이 패키지가 참고하는 패키지
  - ///<reference path/>
    - 지금 이 패키지가 참고하는 파일

### declare

- 실제로 실행되는 구현부는 없는 대신 타입 선언만 하고 싶을 때, 사용

### namespace

- 같은 이름의 타입이 충돌하지 않게 다른 네임스페이스 안에 동일한 이름의 타입을 명시

### 🔴 export = 타입 이해하기

- export = 문법은 타입스크립트에만 있는 문법으로 CommonJS 문법을 사용하기 위해 존재
  - jQuery는 CommJs 모듈 시스템을 지원
    - const $ = require(’jquery’) 가능
    - 다만, 타입스크립트에서는 import $ from ‘jquery’ 로 해야 함

### ⚪ 스크립트 파일과 모듈 파일 이해하기

- 파일 내부에서 최상위 스코프에 import,export(O) ⇒ 모듈 파일
- 없으면 ⇒ 스크립트 파일
  - @types/jquery에서 misc.d.ts에서 $를 import 하지 않아도 쓸 수 있음 ⇒ misc.d.ts 파일은 스크립트 파일이다
  - 스크립트 파일은 기본적으로 모든 항목이 전역이다
    - declare global 하지 않아도 declare global 처럼 동작함
- **동일한 네임스페이스 , 동일한 인터페이스는 서로 합쳐진다**
- **기존 타입 선언을 병합하려면 스크립트 파일이 아니라 모듈 파일 안에 declare module을 선언해야함**
