# Return from constructors solution

```js
function Person (name, age) {
  this.name = name;    
  this.age = age;  
  return 'buz';
}   
function Animal (name, age) {
  this.name = name;
  this.age = age;
  return {name: 'toto', age: 3};
}
const a = new Person('bar', 20);
const b = new Animal('tutu', 2);
console.log(a); // ?
console.log(b); // ?
```

1. new 생성자 함수에서 리턴값이 없으면 this로 바인딩 된 새로 생성된 객체가 리턴된다.
2. new 생성자 함수에서 리턴값이 있고
   1. 리턴값이 객체가 아닌 불린, 숫자, 문자열이라면 이 값을 무시하고 1번 값이 리턴된다.
   2. 리턴값이 객체라면 그 객체가 리턴된다.

