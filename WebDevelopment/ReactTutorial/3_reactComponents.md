# React Components

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

  
