# Code refactoring

## General

+ Moving responsibility, use class component instead of simple functional component if needed.



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

  

  