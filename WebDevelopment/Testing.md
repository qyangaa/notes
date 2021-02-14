### Forms

```javascript
const setValue = "test"
const input = findByTestAttr(component, "input")
input.simulate('change',{
    target: {name: 'fieldName', value: setValue}
})
test(`input value changed to ${setValue}`, ()=>{
    expect(input.props().value).toBe(todoValue)
})
```

```javascript
form.simulate('submit',{
    preventDefault:()=>{}
})
```



### Reducer

```javascript
const posts=[{title: 'test'}]
const newSTate = postReducer(undefined, {
    type: 'GET_POSTS',
    payload: posts
})
expect(newState).toEqual(posts);
```

