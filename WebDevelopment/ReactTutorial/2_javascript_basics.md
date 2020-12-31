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

+ String: 

  + Template literal
  
    ```jsx
    {`/movies/:{movies._id}`}
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

  + change value at index:

    ```javascript
  const index = array.indexOf(item);
    array[index] = {...item};
    array[index].value++;
    this.setState({ array });
    
    ```
    
  + Filter

    ```jsx
    array.filter(item=>item!=1); //delete item from array
    filtered = criteria? 
        array.filter(m=>m.criteria===criteria): array;
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

+ Math:

  + `Math.ceil(..)`
  
+ lodash

  + `const pages = _.range(start, end, step);`
  + `_.slice(items, startIndex)`
  + `_.take()`
  + wrap as lodash object: `_(items).fun(..).fun(..)..`

### Control flow

+ For loop: 

  ```jsx
  for(let item of error.details) error[item.path[0]] = item.message;
  ```

  