# **Return** from **constructors**

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

