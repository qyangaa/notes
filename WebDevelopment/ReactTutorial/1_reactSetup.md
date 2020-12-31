```
npx create-react-app my-app
cd my-app
code -a
npm start
```

+ Bootstrap
  
  + Get examples: https://getbootstrap.com/docs/4.0/examples/
  
+ `npm i lodash`, `import _ from 'lodash'`

+ `npm i prop-types`: enforce type check for classes

  ```jsx
  ClassName.propTypes = {
      item: PropTypes.number.isRequired,
      ...
  }
  ```

+ `npm i` to set up imported folder

+ `npm i react-router-dom`

  ```jsx
  import { BrowserRouter } from `react-router-dom`
  ```

  

+ Query string: `npm i query-string`

+ schema description language and data validator: `npm install joi-browser`

## Shortcuts

`imrc`:`import React, { Component } from 'react';`

`cc`: 

```javascript
class App extends Component {
  state = {  }
  render() { 
    return (  );
  }
}
export default App;
```

+ Zen coding:
  + Generate table: 
    + `table.table>thead>tr>th*4`
    + `tbody>tr>td*4`
    + `td*4`
  + Generate button:
    + `button.btn.btn-danger.btn-sm`