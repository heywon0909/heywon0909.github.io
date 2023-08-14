---
layout: post
title: typescript 제대로 공부하기 - jquery 타입 분석
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image: /assets/img/study/jqueryDt.jpg
sitemap: false
---


zerocho의 타입스크립트 올인원:Part1.기본문법 편2를 듣고 정리한 내용이다.


## topic 🚀

1. @types/jquery 살펴보기
2. commonjs 모듈 타이핑 & escModuleInterop
3. 네임스페이스(namespace)
4. 메서드와 this 타이핑


## jquery 타입 분석 

이제 올인원:Part1을 완강했다 😍
이제 올인원:Part2도 열심히 듣고 배운 내용을 흡수해보자 👩‍🎤

### @types/jquery 살펴보기

npm 사이트에서 jQuery를 검색해보자. 
jQuery 오른쪽 아이콘을 보면, TS가 아닌 DT라고 적혀있다. 
이는 Definitely Types의 약자로, jQuery에 TypeScript를 적용하려면 @types/jQuery를 설치하여 써야한다는 것을 의미한다. 
단, jQuery가 아닌 다른 사람들이 기여하여 type을 만들어 놓았기 때문에 타입에 약간의 오류가 있을 수도 있다.

![200x200](/assets/img/study/jqueryDt.jpg "jQuery는 DT이다.")


@types/jquery의 내용을 분석해보자 
npm으로 @types/jquery를 설치하여 node_modules 안의 폴더를 살펴보자 

![200x200](/assets/img/study/nodemodule.jpg "node_modules안의 @types/jquery 분석")

package.json을 살펴보면 types라는 속성값이 있는데 이때 이 types에 선언된 파일이 ts파일의 메인 types 파일이라고 생각하면된다.

types = index.d.ts이므로 
해당 파일로 들어가면 상세정보를 확인해볼 수 있다.

이 파일이 실제 index.d.ts 파일이다.
아래 보이는 reference는 해당 type을 선언할때 sub 타입 파일들이라고 생각하면된다.


```js
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



### commonjs 모듈 타이핑 & escModuleInterop

index.d.ts파일에서보면 export=jQuery라고 되어있는 부분을 확인해볼 수 있다. 

>'export = jQuery' = 'moudle.export = jQuery'

이 형식은 typeScript에서 CommonJS 형식으로 표기한 방법이다. 


tsconfig.json의 esModuleInterop값 설정에따라(boolean) 모듈 타이핑이 달라지게 된다.

1. esModuleInterop = false;

```js
const $ = require('jQuery'); = import $ = require('jQuery') = import * as $ from 'jQuery'

```

2. esModuleInterop = true;

```js
// 타입
import $ from 'jQuery'
import React from 'react';

```

이렇게 간단하게 변경해서 쓸 수 있게 된다.


### 네임스페이스(namespace)

jquery d.ts 파일들을 살펴보면, namespace JQuery 안에 다양한 type들이 선언되어있는 것을 볼 수 다. 
이는 JQuery안의 타입들이 이미 다른 곳에서 선언이 되어있으므로 같은 이름으로 쓰면 충돌이 발생할 수 있기때문에 고안해낸 방법이다.
JQuery라는 namespace로 감싸줘서, 타입을 선언할때 JQuery.htmlString 이러한 방식으로 쓸 수 있게 된다.

```js
declare namespace JQuery {
    type TypeOrArray<T> = T | T[];
    type Node = Element | Text | Comment | Document | DocumentFragment;

    /**
     * A string is designated htmlString in jQuery documentation when it is used to represent one or more DOM elements, typically to be created and inserted in the document. When passed as an argument of the jQuery() function, the string is identified as HTML if it starts with <tag ... >) and is parsed as such until the final > character. Prior to jQuery 1.9, a string was considered to be HTML if it contained <tag ... > anywhere within the string.
     */
    type htmlString = string;
    /**
     * A selector is used in jQuery to select DOM elements from a DOM document. That document is, in most cases, the DOM document present in all browsers, but can also be an XML document received via Ajax.
     */
    type Selector = string;

    /**
     * The PlainObject type is a JavaScript object containing zero or more key-value pairs. The plain object is, in other words, an Object object. It is designated "plain" in jQuery documentation to distinguish it from other kinds of JavaScript objects: for example, null, user-defined arrays, and host objects such as document, all of which have a typeof value of "object."
     *
     * **Note**: The type declaration of PlainObject is imprecise. It includes host objects and user-defined arrays which do not match jQuery's definition.
     */
    interface PlainObject<T = any> {
        [key: string]: T;
    }

    interface Selectors extends Sizzle.Selectors {
        /**
         * @deprecated ​ Deprecated since 3.0. Use \`{@link Selectors#pseudos }\`.
         *
         * **Cause**: The standard way to add new custom selectors through jQuery is `jQuery.expr.pseudos`. These two other aliases are deprecated, although they still work as of jQuery 3.0.
         *
         * **Solution**: Rename any of the older usage to `jQuery.expr.pseudos`. The functionality is identical.
         */
        ':': Sizzle.Selectors.PseudoFunctions;
        /**
         * @deprecated ​ Deprecated since 3.0. Use \`{@link Selectors#pseudos }\`.
         *
         * **Cause**: The standard way to add new custom selectors through jQuery is `jQuery.expr.pseudos`. These two other aliases are deprecated, although they still work as of jQuery 3.0.
         *
         * **Solution**: Rename any of the older usage to `jQuery.expr.pseudos`. The functionality is identical.
         */
        filter: Sizzle.Selectors.FilterFunctions;
    }
///........

```


### 메서드와 this 타이핑

여기 callback 함수안에서 쓰이는 this는 어떻게 쓸 수 있는 걸까? 

```js
$(['p', 't']).text(function () {
    console.log(this);
    return false;
})
document.querySelector('h1')?.addEventListener('click', function () {
    console.log(this); 
})


```

addEventListener의 type Definition을 들어가보면, this가 선언되어있는것을 볼 수 있다. 

```js
addEventListener<K extends keyof HTMLElementEventMap>(type: K, listener: (this: HTMLHeadingElement, ev: HTMLElementEventMap[K]) => any, options?: boolean | AddEventListenerOptions): void;
```

text메서드도 마찬가지로 type Definition을 들어가보면, this가 선언되어있는것을 볼 수 있다. 

```js
text(text_function: string | number | boolean | ((this: TElement, index: number, text: string) => string | number | boolean)): this;
```

이렇게 이미 this의 타입을 선언해놨기때문에 실제로 사용시 this를 매개변수로 정의하지않아도, this를 사용할 수 있는 것이다. 👍


#### 인수는 생략할 수 없지만, 매개변수는 안쓰면 생략가능하다.

html메서드의 타이핑을 보면 
>html(htmlString_function: JQuery.htmlString |
                              JQuery.Node |
                              ((this: TElement, index: number, oldhtml: JQuery.htmlString) => JQuery.htmlString | JQuery.Node)): this;

이렇게 적혀있다. html 메서드안 callback함수를 인자로, index, oldhtml 매개변수를 받아서 쓰는 함수를 사용하고 있다.
하지만 아래 예시에서처럼
index,oldhtmld은 생략이 가능하다.


하지만, add 함수에서 보면 두번째 매개변수를 생략하여 함수를 호출하면, add('1') // 에러가 난다.
인수는 생략할 수 없으므로 add('1','2'); 인수 개수만큼 명시해줘야한다.


```js

$(tag).html(function(){
    return '<div>hello</div>'
})
function add(x:string,y:string):string{return x + y};

```