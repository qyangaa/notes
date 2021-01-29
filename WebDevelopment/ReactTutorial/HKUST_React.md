

### React Basics

+ Virtual DOM

  + Browser DOM is a browser object 
  + Virtual DOM is a React object 
  + A lightweight representation of the Browser DOM
  + In-memory tree data structure of plain JS objects 
  + Manipulations extremely fast compared to modifying the browser DOM
  + Created completely from scratch on every setState
  + Updating the DON
    Diffing algorithm will detect those nodes that are changed
    - Updates the entire sub-tree if diffing detects that two elements are of different types
    - Using "key" you can hint child elements as stable No need to re-render where keys do not change React Fiber: new reconciliation algorithm in React 16
    - Incremental rendering

  

### Web App Routing
#### Install react-router-dom
Router component: $<$ BrowserRouter>

- Creates specialized history object
- Also <HashRouter $>$ if you are using a static file server
- Enclose your app in BrowserRouter

#### Route Matching
Route matching components: <Route> and <Switch>

- <Route>'s path prop enables specification of the current location's pathname
- <Route>'s component prop specifies the corresponding view for the location
- Using exact attribute ensures that the path must be exactly matched
- <Redirect> enables the default route specification
- <Switch $>$ enables grouping together several routes Will iterate over all its children and find the first one that matches the path

#### Navigation

- Navigation is supported through the <Link> and <NavLink> components:
- <Link> creates links in your application Will render as $<a>$ in the HTML
- <NavLink> also attaches the active class to the link when its prop matches the current location

#### React Router
+ Paths specified as a URL
  Paths can also carry parameter values:
  + e.g., /menu/42 where 42 is a route parameter Route parameters specified in the path specification as a token
  + e.g., path: 'menu/:id' where id is the token

+ Route Parameters
  Route parameters can be specified using a link parameter while specifiyng the link

  + e.g., <Link to\{'/menu/\$\{dish.id\}'\} $>$
    Route passes three props to the component:
  + match, location, history

+ match object provides information about how a <Route path> matched the URL
  - params: an object that contains key/value pair parsed from the URL corresponding to the dynamic segments of the path
  - e.g. if path is specified as /menu/:id, then a path like /menu/42 will result in match.params.id being equal to "42"
  - `<Route path="/menu/: dishId" component={DishWithId} />`
  - ![image-20210120121816226](/home/arkyyang/files/notes/notes/attachments/image-20210120121816226.png)![image-20210120121826859](/home/arkyyang/files/notes/notes/attachments/image-20210120121826859.png)
  - 

  ### Lifecycle Methods

+ Mounting Lifecycle Methods
  Called when an instance of a component is being created and inserted into the DOM:

  + constructor()
  + getDerivedStateFromProps()
  + render()
  + componentDidMount() 
  + An earlier method now deprecated called componentWillMount()

+ Updating Lifecycle Methods
  Called when a component is being re-rendered

  - Can be caused by changes to props or state
  -  getDerivedStatefromprops() 
  - shouldComponentUpdate() 
  - render() 
  - getSnapshotBeforeUpdate() 
  - componentDidUpdate()

## Architecture

### Components

+ Presentational Components
  Mainly concerned with rendering the "view"
  + How things look (markup, styles) Render the view based on the data that is passed to them in props
    Do not maintain their own local state
  + Can be relaxed to maintain only UI state than data

+ Container Components
  Responsible for making things work Data fetching, state updates Make use of presentational components for rendering

  - Can wrap presentational components in wrapping divs Provide the data to the presentational components Maintain state and communicate with data sources

+ Controlled Components
  Make the React component control the form that it renders:
  - Single source of truth
  - Tying the form state to component state
  - Controlled component
  - Every state mutation will have an associated handler function

+ Uncontrolled Components
  - Ideally you should implement forms within controlled components

  + Sometimes this approach may be too tedious
  + Uncontrolled component approach allows you to handle the form data by the DOM itself

### Single Page Applications

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210120121422497.png" alt="image-20210120121422497" style="zoom:67%;" />

+ No need to reload the entire page
- UX like a desktop/native application
- Most resources are retrieved with a single page load
- Redraw parts of the page when needed without requiring a full server roundtrip
+ Challenges in SPA
  Search engine optimization,  Partitioning the responsibility between client and server, Maintaining history Analytics, Speeding up the initial page load

### The Model-View-Controller (MVC) Framework

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210120122113366.png" alt="image-20210120122113366" style="zoom:70%;" />

Software engineering architecture pattern
- Isolation of domain logic from user interface
- Permits independent development, testing and maintenance (separation of concerns)
- Mode
  - manages the behavior and data of the application domain
  - responds to requests for information about its state (usually from the view)
  - responds to instructions to change state (usually from the controller)
  - In event-driven systems, the model notifies observers (usually views) when the information changes so that they can react
- Controller
  - receives user input and initiates a response by making calls on model objects
  - A controller accepts input from the user and instructs the model and viewport to perform actions based on that input

### Model View View-Model (MVVM)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210120122310561.png" alt="image-20210120122310561" style="zoom:67%;" />

- Descendent of MVC
  Sometimes called Model-View-BinderView model
- Abstraction of the view that exposes public properties and commands

- import {createStore} from 'redux';
  import  Declarative data binding

### Flux Architecture

Unidirectional Data flow

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210120122405947.png" alt="image-20210120122405947" style="zoom:67%;" />



## Redux

### Principles

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210120122554727.png" alt="image-20210120122554727" style="zoom:67%;" />

+ Single source of truth
  
+ Single state object tree within a single store 
  
+ State is read-only (only getters, no setters)

  + Changes should only be done through actions 

+ Changes are made with pure functions

  + Take previous state and action and return next state
  + No mutation of the previous state

+ Single store and single state tree enables powerful techniques:
  - Logging
  - API handling
  - Undo/redo
  - State persistence “time-travel debugging"

+ Components:

  - State: stored in plain JS object 
  + Action: 
    - plain JS object with a type field that specifies how to change something in the state 
    + Actions: payloads of information that send data from your application to the store
      - Done through store. dispatch() Plain JS object that must have
        - A type property that indicates the type of action to be performed 
          - Best supported by defining action types as String constants
        - Rest of the object contains the data necessary for the action (payload)
  + Reducer: pure functions that take the current state and action and return a new state
    - Update data immutably (do not modify inputs)
    - Reducers should be able to take the previous state and action and return next state:
      - Do not mutate state 
        - Make a copy and modify the copy before returning it
      - Actions typically handled through a switch statement switching on the action type
      - Return the previous state in the default case
  + Redux Store
    - Holds the current state value
    - Created using createStore() 
    + Supplies three methods:
      - dispatch(): states state update with the provided action object
      - getState(): returns the current stored state value
      - subscribe(): accepts a callback function that will be run every time an action is dispatched
  + React with Redux
    Use the react-redux package for bindings between React and Redux
    - connect(): generates a wrapper "container" component that subscribes to the store 
    - Surround your App root with < Provider> 
      - Takes the store as an attribute 
      - Makes store accessible to all connected components
    - The connect() function takes two optional arguments:
      - mapStateToProps(): called every time store state changes. Returns an object full of data with each field being a prop for the wrapped component
      - mapDispatchToProps(): receives the dispatch() method and should return an object full of functions that use dispatch()

+ Redux Middleware

  + Provides the capability to run code after an action is dispatched, but before it reaches the reducer
    + Third-party extension point
    + e.g., logging, async API calls 
  + Middleware:
    + Forms pipeline that wraps around the dispatch() 
    + Pass actions onward 
    + Restart the dispatch pipeline 
    + Access the store state
  + Middleware typically used for:
    - Inspecting the actions and the state,
    - Modify actions,
    - Dispatch other actions,
    - Stop actions from reaching the reducers, etc. 
  + The applyMiddleware() function:
    - Sets up the middleware pipeline
    - Returns a "store enhancer" that is passed to createStore()

+ Thunk
  In programming, a thunk is a subroutine used to inject an additional calculation in another subroutine
  - Delay a calculation until its result is needed,
  - Insert operations at the beginning or end of the other subroutine

+ Redux Thunk

  + Middleware that allows you to write action creators that return a function instead of an action
  + Can be used to delay the dispatch of an action, or
  + Dispatch only if a certain condition is met
  + Inner function receives the dispatch() and getState() store methods
  + Useful for complex synchronous logic
    - Multiple dispatches
    - Conditional dispatches
    - Simple Async logic 
  + Redux Saga: Uses ES6 generators to control pausable functions
    - Comples async logic
    - Ongoing "background thread" like processing behavior

+ 

  

### Usage

+ `providersReducer.js` (each group of data has its own reducer)

  ```javascript
  import { PROVIDERS } from "../services/data/providerData";
  import * as ActionTypes from "./ActionTypes";
  
  export const providersReducer = (state = PROVIDERS, action) => {
    switch (action.type) {
      case ActionTypes.ADD_PROVIDERS:
        return action.payload;
      default:
        return state;
    }
  };
  
  ```

+ `ActionTypes.js`

  ```javascript
  export const LOAD_PROVIDERS = "LOAD_PROVIDERS";
  ```

+ `ActionCreators.js`

  ```javascript
  import * as ActionTypes from "./ActionTypes";
  
  export const addProviders = (providers) => ({
    type: ActionTypes.ADD_PROVIDERS,
    payload: {
      providers: providers,
    },
  });
  
  export const providersLoading = (isLoading) => ({
    type: ActionTypes.PROVIDERS_LOADING,
    payload: isLoading,
  });
  
  // use async function to dispatch
  export const fetchProviders = async (dispatch) => {
    dispatch(providersLoading(true));
    GetProviders().then((providers) => dispatch(addProviders(providers)));
  };
  ```

  

+ `configureStore.js`:

  ```javascript
  import { createStore } from "redux";
  import { Reducer, initialState } from "./reducer";
  
  export const ConfigureStore = () => {
    const store = createStore(Reducer, initialState);
    return store;
  };
  
  ```

+ `App.js`:

  ```java
  import { Provider } from "react-redux";
  import { ConfigureStore } from "./redux/configureStore";
  
  const store = ConfigureStore();
  
  return(
  <Provider store={store}>
      ...
      </Provider>
  )
  ```

  

+ In Class component: `export default withRouter( connect (mapStateToProps) (Main) )`

+ In functional component:

  ```javascript
  import { useSelector } from "react-redux";
  export default function Providers() {
      const providers = useSelector((state) => state.providers);
  }
  
  ```

  





## Networking

+ Client-Server Communication
  + Network operations cause unexpected delays
  + You need to write applications recognizing the asynchronous nature of communication

### Hypertext Transfer Protocol (HTTP)

- A client-server communications protocol 
- Allows retrieving interlinked text documents (hypertext) World Wide Web.
- HTTP Verbs
  - HEAD
  - GET
  - POST
  - PUT
  - DELETE
  - TRACE
  - OPTIONS
  - CONNECT

![image-20210122185547388](/home/arkyyang/files/notes/notes/attachments/image-20210122185547388.png)

+ HTTP request message:

  + request line: Method	URL	Version
    + $\text { GET /index.html HTTP/1.1 }$
  + headers: Header Field Name: Value ....
    + host: localhost:3000 
    + connection: keep-alive 
    + user-agent: Mozilla/5.0... 
    + accept-encoding: gzip, deflate, sdch
  + blank line
  + body contents

+ HTTP response message:

  + response line: Version	Response_code	status
    + HTTP/1.1	200	OK
  + headers: Header Field Name: Value ....
    + Connection: keep-alive 
    + Content-Type: text/html 
    + Date: Sun, 21 Feb 201606: 01: 43 GMT 
    + Transfer-Encoding: chunked
  + blank line
  + body contents:
    + <html>...</html>

+ HTTP response code:
  $$
  \begin{array}{|l|l|}
  \hline \text { Code } & \text { Meaning } \\
  \hline 200 & \text { OK } \\
  201 & \text { Created } \\
  301 & \text { Moved Permanently } \\
  304 & \text { Not Modified } \\
  400 & \text { Bad Request } \\
  401 & \text { Unauthorized } \\
  403 & \text { Forbidden } \\
  404 & \text { Not Found } \\
  422 & \text { Unprocessable Entry } \\
  500 & \text { Internal Server Error } \\
  505 & \text { HTTP Version Not Supported } \\
  \hline
  \end{array}
  $$
  

### Javascript Object Notation (JSON)

+ collection of name/value pair
+ ordered list of values

```json
Example
{ "promotions": [
        {
        "id": 0,
        "name": "Weekend Grand Buffet",
        "image": "images/buffet.png",
        "label": "New",
        "price": "19.99", "description": "Featuring mouthwatering combination
        }
	]
}
```

#### Setup json-server for dummy test

+ create folder json-server
+ `sudo npm instapp -g json-server`
+ in json-server folder: `json-server --watch db.json -d 2000 -p 3001`
  + -d: delay for network simulation
  + -p: port to use (default 3000)



### Fetch

+ The Fetch API is a modern replacement for XMLHttpRequest
  + Provides an interface for fetching resources (including across the network)
  + Promise based
+ Fetch Abstractions
  - Request: Represents a resource request 
  - Response: Represents the response to a request 
  - Headers: Represents response/request headers, allowing you to query them and take different actions depending on the results
  - Body: Provides methods relating to the body of the response/request, allowing you to declare what its content type is and how it should be handled

```javascript
//Installation
npm install cross-fetct

//Get data (deal with errors)
fetch(baseUrl + 'dishes')
	.then(response => {
    	if(response.ok){
            return response;
        }
    	else{
            var error = new Error('Error'+ response.status+":"+response.statusText);
            error.response = response;
            throw error;
        }
    },
    error=>{
		var errmess = new Error(error.message);
    	throw errmess;
    })
	.then(response => response.json())
	.then(data => console.log(data))
	.catch(error=> console.log(error.message));

//Post data
fetch(baseUrl + 'comments',{
    method: 'POST',
    body: JSON.stringify(newComment),
    headers:{
        'Content-Type':'application/json'
    },
    credentials:'same-origin'
})

// In action
export const fetchDishes = () => (dispatch) =>{
    dispatch(dishesLoading(true));
    
    return fetch(baseUrl + 'dishes')
    		.then( response => response.json())
    		.then( dishes => dispatch(addDishes(dishes)));
}
```

+ Alternatives
  + Axios
  + Superagent



## react forms

+ LocalForm
  + Maps form model to local state of the component
  + Suitable when you don't need form data persistence across component mounting/unmounting 
  + Can still perform form validation using support from react-redux-form













![image-20210122190706952](/home/arkyyang/files/notes/notes/attachments/image-20210122190706952.png)

![image-20210122190731046](/home/arkyyang/files/notes/notes/attachments/image-20210122190731046.png)

![image-20210122190742031](/home/arkyyang/files/notes/notes/attachments/image-20210122190742031.png)

![image-20210122190753833](/home/arkyyang/files/notes/notes/attachments/image-20210122190753833.png)

![image-20210122190813892](/home/arkyyang/files/notes/notes/attachments/image-20210122190813892.png)

![image-20210122190829977](/home/arkyyang/image-20210122190829977.png)

![image-20210122190840709](/home/arkyyang/files/notes/notes/attachments/image-20210122190840709.png)

![image-20210122190854018](/home/arkyyang/files/notes/notes/attachments/image-20210122190854018.png)

![image-20210122190903418](/home/arkyyang/files/notes/notes/attachments/image-20210122190903418.png)

![image-20210122190921054](/home/arkyyang/files/notes/notes/attachments/image-20210122190921054.png)



![image-20210122190938020](/home/arkyyang/files/notes/notes/attachments/image-20210122190938020.png)

![image-20210122190950732](/home/arkyyang/files/notes/notes/attachments/image-20210122190950732.png)





## React Animation

+ React Animation Components
  + A set of react components implemented using reacttransition-group 
  + Provides drop in GPU accelerated animations and wrappers for group effects 
  + Animation components
    + Fade, Transform, FadeTransform
  + Wrapper components
  - Stagger, Random, Loop
+ Libraries: 
  + React-transition-group
    + Components supported:
      + Transition
      + CSSTransition
      + TransitionGroup
  + React-animation-components
    - Manages a set of <Transition> components in a list
    - Automatically toggles the in prop for the components
+ Transition
  + Basics
    + Used to animate the mounting and unmounting of a component 
    + The in prop is used to toggle the transition state
      + When true the component begins the sequence of entering $\rightarrow$ entered state
      + Timeout specifies the duration spent in the entering state
    + entering, entered, exiting, exited 
  + CSSTransition
    + Uses the in prop to decide when to apply the transition classes 
    + Example:
      <CSSTransition key=\{this.props.location.key\} classNames="page"
      timeout $=\{300\}>$
      - Should define .page-enter, .page-enter-active, .page-exit, . page-exitactive CSS classes