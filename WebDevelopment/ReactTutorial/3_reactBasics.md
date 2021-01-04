# React Basics

+ Embed expressions: `<..>{this.function()}</..>`

+ Bootstrap attributes: `<span className="badge badge-primary m-2">`

+ Style attributes: `<span style={this.styles}>`

+ Set attributes dynamically: `<span className={this.getBadgeClasses()}>`

+ Render list: `<ul>{this.state.tags.map(tag=><li key={tag}>{ tag }</li)}</ul>`

+ Handle events:`<button onClick={this.handleIncrement}>`, 
  
  + passing arguments `<button onClick={() => this.handleIncrement(product)}>`
  
+ Bind methods: in constructor `this.handleIncrement.bind(this);`, or use arrow functions

+ Composing components: `{this.state.counters.map(counter => <Counter key={counter.id} />)}`

+ pass children: `this.props.children`

+ **prop vs. state:**

  + `this.setState({ username: 'rstacruz' })`
  + `state={value: this.props.value}`

+ **Raising and handing events**

  + Component that owns the piece should be the one modifying it

  + Remove local state of child class, use `props.item` directly

    ```jsx
    class Counters{
        handleDelete =()=>{
        	const counters = this.state.counters.filter(c=>c.id!==counterId);
            this.setState({ counters: counters});
        }
        render(){return(<Counter 
                            key = {counter.id}
                            onDelete={this.handleDelete}
                            item = {this.state.item}
                            /> )}
    }
    class Counter{
        const { item } = this.props;
        render(){return(<button 
                            onClick={this.props.onDelete(item)}
                            />)}
    }
    ```

+ set classes dynamically:

  ```jsx
  getBadgeClasses(){
      let classes = "badge m-2 badge-";
      classes += this.props.counter.value === 0? "warning": "primary";
      return classes;
  }
  ```

+ Multiple components in sync: put shared data in their common parent component

+ Destructuring arguments: 

  ```jsx
  const{ onReset, counters, onDelete } = this.props;
  ```

  

+ simple functional components (sfc) [with destructuring arguments]

  ```jsx
  const NavBar=({ totalCounters }) =>{
      return(<..>{totalCounters} </..>)
  }
  ```

### Lifecycle Hooks

+ Mount: constructor -> rendered (children rendered) -> mounted 
  + `constructor(props){super(props);..}`
  + `render()`
  + `componentDIdMount()`
+ Update
  + `render()`
  + `componentDidUpdate()`
    + `if(prevProps.item != this.props.item){//Ajex call to server}`
+ Unmount
  + `componentWillUnmount`

### Bootstrap

+ Table sorting: 

  ```jsx
  <th onClick={()=> onSort('title')}>Title</th>
  ```

  

## Navigation



### Route

#### Setup

in `index.js`: 

```jsx
import { BrowserRouter } from `react-router-dom`

ReactDom.render(
	<BrowserRouter>
    	<App />
    </BrowserRouter>,
    ...
)
```

#### Usage

+ `Route` component controls what url can access what component

+ order from the most specific ones to most generic ones

+ use switch to prevent accessing multiple components together

+ Use exact if need exact match
  `<Route path="/" exact component={Home} />`

+ React router wraps a component with the following three objects

  + match 
  + location 
  + history

+ To pass parameters over route components use arrow functions

  To pass all props, use `{...props}`

  `{props => <Products sortBy="newest" {...props} />}`

+ To use parameters passed by route, use `this.props.match.params.something` ..
  Access: `<h1>Product Details - {this.props.match.params.id} </h1>`

+ Generally do not include optional parameters (`/:param?`) in the URL in route   instead include them in the query string
  `<Route path="/posts/:year?/:month?" component={Posts} />`
  Access: `Year: {match.params.year} , Month: {match.params.month}`
  
+ `Redirect` component

  + Add `Redirect to="/not-found"` for any other pages not listed in the route in the end
  + used to relocate resources
    `<Redirect from="/messages" to="/posts" />`

+ Routing can be nested (add in sub-components)

+ Go to specific component with id: `<Route path="/products/:id" component={ProductDetails} />`

+ pass additional props 

  + into `<Route />`, do not add directly, 

    +  use `render` component:

    ```jsx
    <Route
        path="/products"
        render={props => <Products sortBy="newest" {...props} />}
        />
    ```

    + We can also use the render method to protect routes

  + into `<Redirect />`, use the location object in `to=`

    ```jsx
    <Redirect
        to={{
            pathname: "/login",
                state: { from: props.location }
        }}
    ```

    + Useful in redirecting users back to last page after logging in

```jsx
import { Route, Switch, Redirect } from "react-router-dom";
class App extends Component {
  render() {
    return (
      <div>
        <NavBar />
        <div className="content">
          <Switch>
            <Route path="/products/:id" component={ProductDetails} />
            <Route
              path="/products"
              render={props => <Products sortBy="newest" {...props} />}
            />
            <Route path="/posts/:year?/:month?" component={Posts} />
            <Route path="/admin" component={Dashboard} />
            <Redirect from="/messages" to="/posts" />
            <Route path="/not-found" component={NotFound} />
            <Route path="/" exact component={Home} />
            <Redirect to="/not-found" />
          </Switch>
        </div>
      </div>
    );
  }
}


class ProductDetails extends Component {
  handleSave = () => {
    // Navigate to /products
    this.props.history.replace("/products");
  };

  render() {
    return (
      <div>
        <h1>Product Details - {this.props.match.params.id} </h1>
        <button onClick={this.handleSave}>Save</button>
      </div>
    );
  }
}
```

+ In single page application only one part of the page is updated rather than the entire page, use link component from react router dom

```jsx
import { Link } from "react-router-dom";

const NavBar = () => {
  return (
    <ul>
      <li>
        <Link to="/">Home</Link>
      </li>
      <li>
        <Link to="/products">Products</Link>
      </li>
      <li>
        <Link to="/posts/2018/06">Posts</Link>
      </li>
      <li>
        <Link to="/admin">Admin</Link>
      </li>
    </ul>
  );
};
```



### Query

+ String following `?`:

  `www.fake.com/posts?sortBy=newst&approved=true`

```jsx
import queryString from "query-string";

const result = queryString.parse(location.search);
```



+  Value parsed from query strings are always strings
+  Use replace rather than push when redirect user from login page



### Navlink

+ Items on Navbar that redirects to different url

```jsx
<NavLink className="nav-item nav-link" to="/rentals">
	Rentals
</NavLink>

content: (movie) => (
    <Link to={`/movies/${movie._id}`}>{movie.title}</Link>
)
```

### Router props

[reference](https://www.freecodecamp.org/news/hitchhikers-guide-to-react-router-v4-4b12e369d10/)

+ **History**

  The **history** object allows you to manage and handle the browser history inside your views or components.

  - **length**: (number), the number of entries in the history stack
  - **action**: (string), the current action (PUSH, REPLACE or POP)
  - **location**: (object), the current location
  - **push(path, [state])**: (function), pushes a new entry onto the history stack
  - **replace(path, [state])**: (function), replaces the current entry on the history stack
  - **go(n)**: (function), moves the pointer in the history stack by n entries
  - **goBack()**: (function), equivalent to go(-1)
  - **goForward()**: (function,) equivalent to go(1)
  - **block(prompt)**: (function), prevents navigation
  - To have a full reload of the window instead of push back to the Target window, use `window.location ='/'`, instead of `history.push`, redirect component also does not reload the whole page

+ **Match**

  The **match** object contains information about how a **<Route path>** matched the URL.

  - **params**: (object), key/value pairs parsed from the URL corresponding to the dynamic segments of the path
  - **isExact**: (boolean), true if the entire URL was matched (no trailing characters)
  - **path**: (string), the path pattern used to match. Useful for building nested routes**.** We’ll take a look at this later in one of the next articles.
  - **url**: (string), the matched portion of the URL. Useful for building nested links.

+ **Location**

  The **location** object represents where the app is now, where you want it to go, or even where it was.



### Form

#### Validation

+ Use rest operators to get the rest properties of an prop, and you spread operator to add it to outputs:

  ```jsx
  const Input=({name, label, error, ...rest})={
      return (
      <input {...rest} id={name} />)
  }
  ```

+ Render error if error is not empty:

  ```jsx
  {error && <div className="alert alert-danger"> {error} </div>}
  ```

+  Onsubmit method by default reload the page completely. to allow calling for server and prevent this default behavior we need to suppress default in our onsubmit method.

  ```jsx
    handleSubmit = (e) => {
      e.preventDefault();
  
      const errors = this.validate();
      this.setState({ errors: errors || {} });
      if (errors) return;
  
      this.doSubmit();
    };
  ```

#### Get form inputs

do not work with document object in react instead work with abstractions on top of the document object. In vanilla JavaScript we use document get element by ID to get input fields,  instead use the ref object, but still we need to minimize the use of ref

To get form inputs make the form object controlled by the parent state, to do this at an attribute value to the input and make the value this.state. Account. Username

To keep form in sync with the state attribute we need to generate a handle change function to set the state

```jsx
  handleChange = ({ currentTarget: input }) => {
    const errors = { ...this.state.errors };
    const errorMessage = this.validateProperty(input);
    if (errorMessage) errors[input.name] = errorMessage;
    else delete errors[input.name];

    const data = { ...this.state.data };
    data[input.name] = input.value;

    this.setState({ data, errors });
  };

  renderInput(name, label, type = "text") {
    const { data, errors } = this.state;

    return (
      <Input
...
        onChange={this.handleChange}
        error={errors[name]}
      />
    );
  }
}
```



#### Initialization

data with value null or undefined will be seen as uncontrolled element and once we pass values to these uncontrolled element and error will occur. Therefore we should always initialize the properties of our form with empty string or values we get from server, not null or undefined

```jsx
  state = {
    data: { title: "", genreId: "", numberInStock: "", dailyRentalRate: "" },
    genres: [],
    errors: {},
  };
```

#### Error handling

Errors object should never be null, it should be either the error or an empty object

```jsx
  handleSubmit = (e) => {
    e.preventDefault();

    const errors = this.validate();
    this.setState({ errors: errors || {} });
    if (errors) return;

    this.doSubmit();
  };
```

#### Render 

+ Render error if exists

```jsx
{error && <div className="alert alert-danger">{error}</div>}
```



## HTTP service

+ Fake api service: https://jsonplaceholder.typicode.com

### Axios

+ Promise: promises to hold a result for an asynchronous process. 
  + initialized in pending state 
  
  + successful: goes to a resolved state 
  
  + failure: to reject state
  
  + Life cycle of a request includes request, post, 
  
    ```jsx
    const promise = axios.get('https://...');
    ```
  
    
  
+ Use await promise to get the response of the promise object and at async keyword in front of the function which has await method

  ```jsx
  async componentDidMount(){
      const response = await promise;
  }
  ```

+ Double square bracket means internal property that we cannot access by dot method

+ Get data:

  ```jsx
  const { data: posts} = await axios.get(apiEndpoint)
  ```

+ Update data: , put method requires to pass in the entire object, patch method requires to pass in only the intended field

  ```jsx
  handleAdd = async() =>{
      const obj = {title:'a', body:'b'};
      const {data: post} = await axios.post(apiEndpoint, obj);
  }
  handleUpdate = async post =>{
      post.title = "UPDATED";
      const {data} = await axios.put(apiEndpoint+'/'+post.id, post);
      
      const posts=[...this.state.posts];
      const index = posts.indexOf(post);
      post[index]={...post};
      this.setState({ posts });
      
  }
  ```

+ Delete data:

  ```jsx
  handleDelete = async post =>{
      await axios.delete(apiEndpoint+'/'+post.id);
      const posts = this.state.posts.filter(p => p.id !== post.id);
  }
  ```

+ Optimistic vs. pessimistic updates:

  + Pessimistic updates call the server first and then update the local state, optimistic is the reversed it assumes that most of the time server calls are successful. 

  + Optimistic implementation give the user the illusion of fast application. But we need to be able to reverse to the original state if the car fails using a try catch block

    ```jsx
      handleDelete = async post => {
        const originalPosts = this.state.posts;
    
        const posts = this.state.posts.filter(p => p.id !== post.id);
        this.setState({ posts });
    
        try {
          await http.delete(config.apiEndpoint + "/" + post.id);
        } catch (ex) {
          if (ex.response && ex.response.status === 404)
            alert("This post has already been deleted.");
          this.setState({ posts: originalPosts });
        }
      };
    ```

+ Expected and unexpected errors

  + Expected: client error (wrong url, wrong input), bewteen 400 and 500

    + give specific error message
    + 404: not found
    + 400: bad request

  + Unexpected error: network, server, db, bug

    + Give generic error message

    + Log them, use interceptors to deal with them

      ```jsx
      axios.interceptors.response.use(null, error => {
        const expectedError =
          error.response &&
          error.response.status >= 400 &&
          error.response.status < 500;
      
        if (!expectedError) {
          logger.log(error);
          toast.error("An unexpected error occurrred.");
        }
      
        return Promise.reject(error);
      });
      ```

    + logging as service provider to log errors, [sentry.io](http://sentry.io/)
  
+ To call protected API endpoints, in HTTP service, use axial. Defaults.headers. Common to set a common header for all the requests

  ```jsx
  axios.defaults.headers.common["x-auth-token"] = jwt;
  ```

  

### Code refractor

+ Place URLs such as `apiEndpoint` in a separate `config.json` file

  ```json
  {
    "apiEndpoint": "https://jsonplaceholder.typicode.com/posts"
  }
  
  ```

+ Extract `httpService.js` and `logService.js` (with react-toastify installed)

  ```jsx
  import axios from "axios";
  import logger from "./logService";
  import { toast } from "react-toastify";
  
  axios.interceptors.response.use(null, error => {
    const expectedError =
      error.response &&
      error.response.status >= 400 &&
      error.response.status < 500;
  
    if (!expectedError) {
      logger.log(error);
      toast.error("An unexpected error occurrred.");
    }
  
    return Promise.reject(error);
  });
  
  export default {
    get: axios.get,
    post: axios.post,
    put: axios.put,
    delete: axios.delete
  };
  
  ```

  ```jsx
  import Raven from "raven-js";
  
  function init() {
    // Raven.config("ADD YOUR OWN API KEY", {
    //   release: "1-0-0",
    //   environment: "development-test"
    // }).install();
  }
  
  function log(error) {
    // Raven.captureException(error);
  }
  
  export default {
    init,
    log
  };
  
  ```

  



## Authentication and Authorization

+ Senarios
  + JSON web tokens
  + Calling protexted APIs
  + Showing/ hiding elements
  + Protecting routes
  
+ resources:

  + jwt.io: website debug tool to decode JSON web tokens
  + development package: jwtDecode from 'jwt-decode'

+ Connection:

  + Remote: if the user sent valid validation information the server will return a JSON token back to the user

  + Local: Use local storage object to store the JSON token

    + inspect local storage in the application tab of developer tools

    ```javascript
    localStorage.setItem(tokenKey, jwt);
    localStorage.removeItem(tokenKey);
    const jwt = localStorage.getItem(tokenKey);
    ```



## Deployment

### Front end

#### Environmental variables

+ Use .env file to store environmental variables, access in JavaScript files with `process.env`, in the `.env` file use key in following convention:

  ```
  REACT_APP_NAME=Vidly in Dev
  REACT_APP_VERSION=1
  ```

  

+ Every time we update environment files we need to restart the application
  The variable representing the environmental variables are substituted by the real variable value during build time

#### Build

```bash
npm run build # generate deploy file
npm i -g serve
serve -s build # serve locally
```





### Backend

#### Heroku: Cloud backend server

Heroku:

https://dashboard.heroku.com/apps/vidly-arky/deploy/heroku-git

```bash
sudo snap install --classic heroku
heroku login
heroku create
heroku git:remote -a vidly-arky
git push heroku master
heroku open
heroku logs --tail
```



#### mlab: remote database hosting



#### node module gitignore

```
.DS_Store
node_modules/
logfile.log
```

heroku config:set vidly_db=mongodb+srv://vidly-admin:1234@cluster0.pmplg.mongodb.net/vidly?retryWrites=true&w=majority