# 如何在 JavaScript 类中创建私有成员

> 原文：<https://javascript.plainenglish.io/private-member-in-javascript-class-2359ef666aaf?source=collection_archive---------0----------------------->

![](img/59f004d3410b6bb8f6f7d4bdf4f8a241.png)

首先，一直到 ES6，官方都不支持 JavaScript 类中的私有成员。

然而，有几种方法可以让*表现得好像我们有一个*。

# 关闭

JavaScript 的闭包自然成为变量所在的“作用域”。如果我们在正确的地方定义了一个类成员，我们就可以拥有私有类成员。

有三种方法可以做到这一点。

## `1\. Constructors`

```
function ConstructorCar(initValue) {
   this.publicMember = initValue;
   let _privateMember = initValue; //private member
   this.getPrivateMember = function() { return _privateMember; }
   this.setPrivateMember = function(v) { _privateMember = v; }
}ConstructorCar.prototype.drive = function() {
  //this._privateMember undefined
  console.log(“Drive! “+this._privateMember); 
}const car2 = new ConstructorCar(100);
console.log(“getPrivateMember: “+car2.getPrivateMember());
car2.drive();
```

## 2.班级

```
class ClassCar {
 constructor(mileage) {
   this.mileage = mileage; //this is public
   var privateMember = mileage;//this is private
   this.getPrivateMember = function() { return this.privateMember; }
   this.setPrivateMember = function(v) { this.privateMember = v; }
 }
 drive() {
   this.mileage ++;
   console.log(‘Drive! ‘+this.mileage);
   console.log(‘privateMember? ‘+this.privateMember); //undefined
 }
}const car1 = new ClassCar(100);
console.log(“privateMember:”+car1.privateMember);//undefined
car1.getPrivateMember();
car1.drive();
```

## 3.工厂

```
//Factory
let FactoryCarProto = (function() {
   var mileage = 0;
   var _privateMember = 0;
   const FactoryCarProto = {
       mileage,
       setMileage(m) {
          this.mileage = m;
       },
       drive() {
          this.mileage ++;
          console.log(‘Drive !’);
       },
       setPrivateMember(v) {
          _privateMember = v;
       },
       getPrivateMember() {
          return _privateMember;
       }
   };
   return FactoryCarProto;
})();function factoryCar() {
   return Object.create(FactoryCarProto);
}const car3 = factoryCar();
car3.drive();
car3._privateMember; //undefined
car3.setPrivateMember(100);
car3.getPrivateMember();
```

闭包方法有一个缺点，对于访问私有数据的方法，它们必须在构造函数中创建，这意味着我们在每个实例中都要重新创建它们。会对内存和性能产生影响。

# 使用 WeakMap

这种方法建立在闭包方法之上。

这里，我们使用作用域变量方法创建一个私有 WeakMap，然后使用该 WeakMap 检索与此相关的私有数据。

这比作用域变量方法更快，因为所有的实例可以共享一个 WeakMap，所以我们不需要每次创建一个实例都重新创建方法。

```
let Person = (function () {
 let privateProps = new WeakMap(); class Person {
   constructor(name) {
     this.name = name; // this is public
     privateProps.set(this, {age: 20+name}); // this is private
   } greet() {
     console.log("Hello: "+privateProps.get(this).age);
   } getAge() {
     return privateProps.get(this).age;
   } setAge(age) {
      privateProps.set(this, {age: age+name});
   } }
 return Person;})();
let joe = new Person(‘Joe’);
console.log(“joe.age? “+joe.age); //undefined;
joe.greet();
```

如果你认为 WeekMap 方式很难看，我们的下一个方法甚至更糟...

# 使用限定范围的符号

我们可以使用符号作为属性名，如下所示。

```
 let ClassCar = (function() {
   let privateMemberKey = Symbol(‘privateMember’);
   class ClassCar {
   constructor(mileage) {
      this.mileage = mileage; //this is public
      this[privateMemberKey] = mileage;//this is private
   }
   drive() {
      this.mileage ++;
      console.log(‘Drive! ‘+this.mileage);
      console.log(‘privateMember? ‘+this[privateMemberKey]);
   }
 }
 return ClassCar;
})();const car1 = new ClassCar(100);
car1.drive(); 
```

但是，有一种方法可以绕过它:

```
//this will give out private member keysObject.getOwnPropertySymbols(car1)
```

所以不是真的 100% `private`。

# 厌倦了所有这些变通办法？

好消息！

ESnext 来了，JS 终于有了对私有成员的支持！

```
class MyClass {
  a = 1;          // .a is public
  #b = 2;         // .#b is private
  static #c = 3;  // .#c is private and static
  static d = 4;
  x = 100;
  incB() {
    this.#b++;
  }
  getB() {
    return this.#b;
  }
  getC() {
    //this.#c ++; //this will error out
    return this.#c;
  }
  /*#incX() { //this won't work
    this.x++;
  }*/
}MyClass.d; //4
var myc = new MyClass();
myc.getC(); // 3
```

# 然而…

ESnext 仍然不支持私有函数——`#incX`无法工作。

永远记住，不是所有的浏览器都支持新标准(是的，我说的就是你)。