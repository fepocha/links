https://devblogs.microsoft.com/typescript/announcing-typescript-3-8-rc/
타입스크립트 3.8에서 뭐가 바꼈나... @juyeonH 작성

- type만 import 가능

```
import type { SomeType } from './some-module.ts'; 
```

---------------  

- ECMAScript’s private fields(#) 지원

```
class Person {
 #name: string; constructor(name: string) {
 this.#name = name;
 }
}
```

-   \#(private fields)는 보통 타입스크립트에서 사용하는 private modifier와 다름.

  -> 모든 private field는 걔를 가지는 class 내에서 unique한 이름을 가진 존재여야 함.
-> 그 class 밖에서는 절대 못씀. (hard privacy: subclasses에서 얘를 덮어쓸 수 없음.)
-> private field 대신 typescript의 public이나 private를 쓸 수 없음.

----------

- export * as ns Syntax(ECMA Script 2020) 

```
import * as utilities from "./utilities.js";
export { utilities };
export * as utilities from "./utilities.js";
```

-> import할 때 요 패턴이 편함? 이제 export할 때도 이 패턴 지원!

--------

- Top-Level await

-> 원래 await 사용하려면 async function 안에서만 사용 가능했음. 그런데 이제 module top level에서 사용 가능!! 대박!!

```
const response = await fetch("...");
const greeting = await response.text();
console.log(greeting);// Make sure we're a module
export {};
```

- 어떤 모듈의 top level에서만 사용 가능(!) -> 파일에 import 또는 export가 있어야만 타입스크립트가 모듈로 인식하기 때문에 export {};를 boilerplate로 사용해야할 수도 있음.
- 요거는 target compiler option이 es2017이상이어야 사용 가능. (즉, 아직 제한적인 기능)
- module과 target을 위해 es2020 지원

-----

- 기존 public, private, protected 키워드 쓰는 대신에 주석으로 맨 상단에 @ts-check라고 쓰고 @private, @public, @protected, @readonly 키워드 사용 가능

```
// @ts-checkclass Foo {
 constructor() {
   /** @private */
   this.stuff = 100;
 } printStuff() {
   console.log(this.stuff);
 }
}new Foo().stuff;
```

-----

- watchOptions

Compiler options 대신 watch options 사용을 통해 특정 파일, 디렉토리에 일정 시간 간격으로 트래킹할 수 있도록 해주는 몇 가지 옵션들을 제공 (문서 참조)

----------

- assumeChangesOnlyAffectDirectDependencies

assumeChangesOnlyAffectDirectDependencies라는 새로운 옵션 지원 -> 이 옵션을 키면 타입스크립트에서 어떤 파일에 영향을 끼칠 가능성이 있는 모든 파일들에 대해 rechecking/rebuilding하는 것을 피함(직접적으로 import된 파일들이 변경되었을때만 recheck/rebuild함)

--------

- type-checker가 더 엄격해짐
- Index signitures에서...

```ts
let obj1: { [x: string]: number } | { a: number };
obj1 = { a: 5, c: 'abc' }
// let obj1: { [x: string]: number };로 바꾸면 기존 경우에도 에러가 나지만 
// let obj1: { [x: string]: number } | { a: number };의 경우에는 에러가 안났음. 
// 왜냐면 {a: 5, c: 'abc'}에서 a:5가 { [x: string]: number } 조건을 만족하고 
// 또는 { a: number } 조건을 만족하고 있기 때문임.// 근데 새로운 타입스크립트는 위 경우도 에러로 검사함.


let obj2: { [x: string]: number } | { [x: number]: number };
obj2 = { a: 'abc' };
// 이 경우에도 기존 타입스크립트에서는 에러가 안났지만 이제는 에러가 남.
```

---------------
noImplicitAny 속성을 킨 상태에서는 더 이상 object을 any로 취급하지 않고 non-primitive object로 취급함.
