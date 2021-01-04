# Code refactoring

## General

+ Moving responsibility, use class component instead of simple functional component if needed.

+ organize import statement in some kind of structure, 

  + first external libraries 
  + second components 
  + last CSS modules

+ If one name or string appears in multiple places then we need to refactor it. 

+ Bi-directional dependencies means that two modules import from each other, only make the last car module depend on the more core module. 

  + To resolve the dependency reverse the functions. 

  + For example a get function can be reversed to a set function called from the other side

    ```jsx
    http.setJwt(getJwt());
    ```

    



## Component Based

### Table

- Put parameters to indicate which columns/ directions to sort with table rather than parent component

- Separate Heather and body into different components

- We cannot access nested property is using square brackets , use `get()` of lodash

- For small components in a table, past them as objects with content property and the that property is what we want to render

  ```jsx
  {
      key: "like",
          content: (movie) => (
              <Like liked={movie.liked} onClick={() => this.props.onLike(movie)} />
          ),
  },
  ```

  

- For a simple functional component destructing the props in the parameters rather than in the code block

### Authentication

+ key "token"  should be stored in auth service only
+ All the local storage methods should be refactored into authservice





