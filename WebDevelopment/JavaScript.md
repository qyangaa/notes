## Cheatsheet

### JavaScript Built-in APIs

| Array   | .push(e)                   | .pop()            | .unshift(e)   | .shift()       | .slice(start, end)          | .concat()     | .splice(start, numRmv, e1, e2..) |
| ------- | -------------------------- | ----------------- | ------------- | -------------- | --------------------------- | ------------- | -------------------------------- |
|         | .sort(sortby)              | .filter(filterby) | .reverse()    | .join(',')     |                             |               |                                  |
| String  | .concat(s2)                | .slice(l, r)      | .replace(o,n) | .split(',', n) | .toLowerCase() .toUpperCase |               |                                  |
|         | .indexOf(c)                | .lastIndexOf(c)   | .match(v)     |                |                             |               |                                  |
| Map     | new Map(arr)               | .has(key)         | .get(key)     | .set(k,v)      | .delete(key)                |               |                                  |
| Set&Map | .keys()                    | .values()         | .entries()    | .forEach()     | .size                       | .size.clear() |                                  |
| Set     | new Set(arr)               | .add(v)           | .delete(v)    | .has(v)        |                             |               |                                  |
| JSON    | .parse(json)               | .stringify(obj)   |               |                |                             |               |                                  |
| Proxy   | new Proxy(target, handler) |                   |               |                |                             |               |                                  |
| Reflect |                            |                   |               |                |                             |               |                                  |





+ 
+ primitive types:

```javascript
String, Number, BigInt, Boolean, Undefined, Null, Symbol
```

+ void function

```html
<a href="javascript:void(document.form.submit())">
Click here to submit
</a>
```

+ Rest and spread operator, object destructuring

```javascript
function f(...args){} //rest
let obj = {...obj1, ...obj2}; //spread
//destructring
const {att1: alias1, att2: alias2} = targetClass
const [first,second,third,fourth] = arr;
```



+ Loop

```javascript
for (let i = 0; i<9; i++){
    break;
    continue;
}
for (const key in obj){} // enumerable, keys
for (const item of array){} //iterable, values (includes enumerable)
while(){}
do{} while()
```

+ Conditional

```javascript
if(){} else if(){} else{}
switch(a){
    case 1: ;
    case 2: ;
    default: ;
}
```

+ `with(object){statements}`: set default object

+ Arguments object

  ```javascript
  function func1(a, b, c) {
    console.log(arguments[0]);
    // expected output: 1
  }
  ```

+ Built-in functions

```javascript
isNaN();//convert to Number type and check is == NaN
isNaN gives true: 'hello', undefined
isNaN gives false: 345, '1', true, false
parseFloat(str); parseInt(str)
```

+ Object

```javascript
var s = {
  firstName: "John",
  lastName : "Doe",
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};

function personFullName() {
    return this.first + ' ' + this.last;
}

function person(name) {
  this.name = name;
  this.fullName = personFullName;
}
s = new person("Simon")

```

#### Built in objects

+ Root

```javascript
// constructor
var test=new Array();
if (test.constructor==Array) {document.write("This is an Array")};
//prototype
employee.prototype.salary=null;
//methods
eval(str); toSource(); toString(); valueOf();
document.write(arr.toString());
```

+ Array

```javascript
var arr = new Array(3);
arr[0] = '1';
arr.conact(arr2)
arr = eval(string);
arr.join(",") + "<br/>"
arr.pop(), arr.push(), arr.reverse();
arr.shift() // remove and return first element
arr.slice(start, end) // excluding end
sort(sortby);
splice(startIdx, numElementsToRmv, element1, element2); //remove and add new elements to an array
unshift(newElement1, newElement2) // add to beginning
const newarr = arr.filter(word => word.length > 6);
```

+ Day

```javascript
Date(); //return date and time
let d = new Date();
d.getDate();getDay(); getMonth(); getFullYear(); getHours(); getMinutes(); getSeconds(); getMilliseconds()
getTime(); //in milliseconds
getTimezoneOffset(); getUTCDate();
Date.parse("Jul 8, 2005") //  milliseconds since January 1, 1970
d.setDate(); setMonth(month, day) ....

```

+ Image Object

```javascript
imageName = new Image([width, height])
imageName.onabort = handlerFunction
imageName.onerror = handlerFunction
imageName.onload = handlerFunction
imageName.boader(); complete; height; hspace; vspace; width; src; name; ...
```

+ Math, number, coersion

![image-20210204124334929](/home/arkyyang/files/notes/notes/attachments/image-20210204124334929.png)

```javascript
//Math.
LN2; LN10; PI; LOG2E; SQRT2
abs(x); acos; asin; ceil; exp; floor; log; max;min; round; sin; sqrt; pow(x,y); random() //0-1
var quotient = Math.floor(y/x); //integer division
var remainder = y % x;
** // exponential
//Number.
MAX_VALUE; MIN_VALUE; NaN
n.toString();
```

+ String

```javascript
s = new String("AAA")
s.length; 
charAt(idx); concat(s1,s2..); s1.concact(s2)
fontcolor(color); fontsize(size); indexOf(searchValue, fromidx); lastIndexOf(searchValue, fromidx); match(searchvalue); replace(findStrig, newString); search(s)->idx; slice(start, end); split(separator, howmany); substr(start, length); substring(start, stop); toLowerCase(); toUpperCase()
```

+ Event

```javascript
onchange; onclick;oncbclick,onerror; onfocus; onkeypress; onkeydown; onkeyup; onload; onmounsedown; onmousemove; onmouseout; onmouseover; onmouseup; onreset; onresize; onselect; onsubmit
```

+ Promise
  + eventual completion (or failure) of an asynchronous operation and its resulting value.
  + pending, fulfilled, rejected, settled (fulfilled or rejected)

```javascript
const p = new Promise((resolve, reject)=>{
    setTimeout(()={
        if(..){
            resolve(..);
        }else{
        reject('...');
    }
    },2000)
})
p.then((res)=> {fres(res)}).catch((err)=>{ferr(err)})
```

+ this keyword

![image-20210204131149118](/home/arkyyang/files/notes/notes/attachments/image-20210204131149118.png)



### Function

```javascript
functionObjectName = new Function ([arg1, arg2, ... argn], functionBody)
// Immediately invoked function:
(function(){ 
})();
//higher order function
function higherOrder(fn) {
  fn();
}
higherOrder(function() { console.log("Hello world") }); 
function higherOrder2() {
  return function() {
    return "Do something";
  }
}   
var x = higherOrder2();
x()   // Returns "Do something"
```

+ Scope:

```javascript
var: global; let, const: block/ functional scoped //inside {}
```

+ Temporal Dead Zone: can't access `let, const` variable before declaration

+ call(), apply(), bind()

```javascript
var person = {
    name: "";
    saySomething: function(message){
  return this.name + " is " + message;
	}
}
       
var person4 = {name:  "John"};

person.getAge.call(person4, "awesome");
saySomething.apply(person4, ["awesome"]);
var say = person.saySomething.bind(person2);
say();
```

+ currying:

```javascript
function add (a) {
  return function(b){
    return a + b;
  }
}
const add3 = add(3)
add3(4)
```

+ Closure

```javascript
function randomFunc(){
  var obj1 = {name:"Vivian", age:45};

  return function(){
    console.log(obj1.name + " is "+ "awesome"); // Has access to obj1 even after the randomFunc function is executed

  }
}

var initialiseClosure = randomFunc(); // Returns a function

initialiseClosure(); //“Vivian is awesome”
```

+ callback: function that will be executed after another function gets executed.

```javascript
function operationOnSum(num1,num2,operation){
  var sum = num1 + num2;
  operation(sum);
}

operationOnSum(3, 3, divideByHalf); // Outputs 3
```

+ Memorization: use `var cache={}` inside function as cache

```javascript
function memoizedAddTo256(){
  var cache = {};

  return function(num){
    if(num in cache){
      console.log("cached value");
      return cache[num]

    }
    else{
      cache[num] = num + 256;
      return cache[num];
    }
  }
}
```

+ Arrow function
  + General function: this keyword always refers to the object that is calling the function.
  + Arrow function: inherits its value from the parent scope

+ Class:
  + syntactic sugars for constructor functions.
  + Unlike functions, classes are not hoisted. A class cannot be used before it is declared.
  + A class can inherit properties and methods from other classes by using the extend keyword.
  + All the syntaxes inside the class must follow the strict mode(‘use strict’) of javascript. Error will be thrown if the strict mode rules are not followed.

```javascript
function Student(name,rollNumber,grade,section){
  this.name = name;
  this.rollNumber = rollNumber;
  this.grade = grade;
  this.section = section;
}
Student.prototype.getDetails = function(){
  return 'Name: ${this.name}, Roll no: ${this.rollNumber}, Grade: ${this.grade}, Section:${this.section}';
}
class Student{
  constructor(name,rollNumber,grade,section){
    this.name = name;
    this.rollNumber = rollNumber;
    this.grade = grade;
    this.section = section;
  }

  // Methods can be directly added inside the class
  getDetails(){
    return 'Name: ${this.name}, Roll no: ${this.rollNumber}, Grade:${this.grade}, Section:${this.section}';
  }
}
```

+ Generator functions

```javascript
function* genFunc(){
  yield 3;
  yield 4;
}
genFunc(); // Returns Object [Generator] {}
genFunc().next(); // Returns {value: 3, done:false}
```

+ WeakSet and WeakMap
  + object inside the weakset does not have a reference, it will be garbage collected.
  + Unlike Set, WeakSet only has three methods, **add()** , **delete()** and **has()** .





### Prototypes

+ **A prototype is a blueprint of an object.** Prototype allows us to use properties and methods on an object even if the properties and methods do not exist on the current object.

```javascript
// Prototype: add new properties to all objects of a constructor outside of constructor
Person.prototype.fullNameReversed = function() {
  return this.last + ', ' + this.first;
}


function Person(first) {
  return {
    first: first,
  }
}
s = Person("Simon")

```



### Work with browser

```javascript
document.getElementById("demo").innerHTML = person;
```



![image-20210204124600010](/home/arkyyang/files/notes/notes/attachments/image-20210204124600010.png)

![image-20210204124611120](/home/arkyyang/files/notes/notes/attachments/image-20210204124611120.png)

![image-20210204124645487](/home/arkyyang/files/notes/notes/attachments/image-20210204124645487.png)





### Coding questions

```javascript
function func1(){
  setTimeout(()=>{
    console.log(x);
    console.log(y);
  },3000);

  var x = 2;
  let y = 12;
}

func1(); // 2, 12
//even though let variables are not hoisted, due to async nature of javascript, the complete function code runs before the setTimeout function. Therefore, it has access to both x and y.
```

```javascript
function func2(){
  for(var i = 0; i < 3; i++){
    setTimeout(()=> console.log(i),2000);
}}

func2(); //3 3 3
//variable declared with var keyword does not have block scope. Also, inside the for loop, the variable i is incremented first and then checked.
function func2(){
  for(let i = 0; i < 3; i++){
    setTimeout(()=> console.log(i),2000);
}}

func2(); //0 1 2

function func2(){
  for(const i = 0; i < 3; i++){
    setTimeout(()=> console.log(i),2000);
}}

func2(); //0
```

```javascript
(function(){
  setTimeout(()=> console.log(1),2000);
  console.log(2);
  setTimeout(()=> console.log(3),0);
  console.log(4);
})();//2 4 3 1 (setTimeout always after complete function)
```

```javascript
let x= {}, y = {name:"Ronny"},z = {name:"John"};

x[y] = {name:"Vivek"};
x[z] = {name:"Akki"};

console.log(x[y]);
//Output will be {name: “Akki”}.Writing x[y] = {name:”Vivek”} , is same as writing x[‘object Object’] = {name:”Vivek”} ,
```

```javascript
function runFunc(){
  console.log("1" + 1); // "11"
  console.log("A" - 1); //NaN
  console.log(2 + "-2" + "2"); //"2-22"
  console.log("Hello" - "World" + 78); //NaN
  console.log("Hello"+ "78"); //Hello78
}
```

```javascript
var x = 23;

(function(){
  var x = 43;
  (function random(){
    x++;
    console.log(x);
    var x = 21;
  })();
})();

//equivalent to
function random(){
  var x; // x is hoisted
  x++; // x is not a number since it is not initialized yet
  console.log(x); // Outputs NaN
  x = 21; // Initialization of x
}
```

```javascript
  let hero = {
    powerLevel: 99,
    getPower(){
      return this.powerLevel;
    }
  }
  
  let getPower = hero.getPower; //func
  
  let hero2 = {powerLevel:42};
  console.log(getPower());//undefined
  console.log(getPower.apply(hero2)); //42
```

```javascript
  const a = function(){
    console.log(this); //undefined, window; 
  
    const b = {
      func1: function(){
        console.log(this);  //b
      }  
    }
  
    const c = {
      func2: ()=>{
        console.log(this); //undefined, this of arrow function refers to the global object.
      }
    }
  
    b.func1();
    c.func2();
  }
  
  a();
```

```javascript
  const b = {
    name:"Vivek",
    f: function(){
      var self = this;
      console.log(this.name); //"Vivek", called by b
      (function(){
        console.log(this.name); //undefined, called by f
        console.log(self.name); //"Vivek"
      })();
    }
  }
  
  b.f();
  
```

```javascript
function binarySearch(arr, val){
  let l = 0;
  let r = arr.length;
  let m;
  while(l<r){
    m = Math.floor(l+(r-l)/2);
    if(arr[m] === val){
      return m;
    }else if(arr[m]>val){
      r=m;
    }else{
      l=m+1;
    }
  }
  return -1;
}
```

```javascript
function rotateRight(arr, rotations){
  if(rotations==0) return arr;
  for (let i=0;i<rotations;i++){
    let element = arr.pop();
    arr.unshift(element);
  }
  return arr;
}
```

