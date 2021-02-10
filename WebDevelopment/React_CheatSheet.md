## Hooks

#### UseReducer

```jsx
const initialState = 0;
const reducer = (state, action)=>{
    switch(action){
        case 'increment':
            return state + 1
        default:
            return state
    }
}
const [count, dispatch] = useReducer(reducer, initialState)

<button onClick={()=> dispatch('increment')}>..</button>
```

