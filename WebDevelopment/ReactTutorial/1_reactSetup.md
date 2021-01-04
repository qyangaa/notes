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

+ Http service: `npm i axios`

+ Nice-looking toasts `npm i react-toastify`

+ Backend service runner: Postman



### Node.js

- delete **bcrypt** from the dependencies: `npm uninstall bcrypt`
- install **bcryptjs**: `npm i bcryptjs`
- replace instances of bcrypt in routes *auth.js* and *users.js* with:

```
const bcrypt = require('bcryptjs')
```

seed the vidly database with `node seed.js` and all three collections should appear. Still taking the course but when I reference the API from postman (`http://localhost:3900/api/movies`) I get the movies results back with a 200 status code.

https://github.com/mosh-hamedani/vidly-api-node/issues/10

mongod --port 27018

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