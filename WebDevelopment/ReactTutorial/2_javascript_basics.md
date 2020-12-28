# JavaScript Basics

+ Scope: var[function], let[block], const[block]

+ Objects

  ```javascript
  const person = {
      name:'Arky',
      age:'18',
      walk() {}
  }
  person.name=''
  const targetMember = 'name';
  person[targetMember.value] = 'John';
  const walk = person.walk.bind(person);//binding
  walk();
  const {name, age} = person;//object destructuring
  
  ```

+ Arrow function:  (all `this` in arrow functions are object scope)

  ```javascript
  const func = (params) => {return ...;}
  const func = param => return ...;
  ```

+ Array

  ```java
  const jobs=[
      { id:1, isActive: true},
      { id:2, isActive: false},
  ];
  const activeJobs = jobs.filter(job => jobs.isActive);
  jobs.map(function(job){
      return '<li>'+ job + '</li>';
  });
  jobs.map(job=>`<li>${job}</li>`);
  
  const first=[1,2,3];
  const second = [4,5,6];
  const combined = [...first,...second];//spread operator for array
  const combined = {...first,...second, locaion:'China'}; //spread operator for objects
  ```

+ Class:

  + `export default` allows import with `import Class from ..`

  ```javascript
  import {Person} from './person'
  
  export default class Teacher extends Person{
      constructor(name, degree){
          super(name);
          ...;
      }
      teach(){...;}
  }
  const teacher = new Teacher("Mosh");
  ```

  