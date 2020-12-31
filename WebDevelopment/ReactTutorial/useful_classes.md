### Useful classes

```jsx
const ListGroup = props =>{
    const { items, textProperty, valueProperty, selectedItem, onItemSelect} = props;
    return(
    	<ul className = "list-group")>
        	{items.map(item=>(
            	<li key={item[valueProperty]}
                    onClick={()=>onItemSelect(item)}
                    className ={item==={
                        selectedItem? "list-group-item active":"list-group-item"
                    }}>
                    {item[textProperty]}
                </li>
            ))}
        </ul>
)};

ListGroup.defaultProps = {
    textProperty:"name",
    valueProperty:"_id"
}

export default ListGroup;
```

### 



### Counter

```javascript
class Counter extends Component{
	state = {
        count: 1
        tags: ['1','2','3']
    };
	styles={
        fontSize:50,
        fontWeight:"bold"
    }

	constructor{
        super();
        this.handleIncrement.bind(this);
    }
	render(){
        return(
        	<div>
            	<span style={this.styles} className="badge badge-primary m-2">{this.formatCount()}</span> 
				<button className="btn btn-secondary btn-sm">Increment</button>
			    <span className={this.getBadgeClasses()}>{this.formatCount()}</span> 
				<ul>
                    {this.state.tags.map(tag=><li key={tag}>{ tag }</li)}
                </ul>
				<button onClick={() => this.handleIncrement(product)}>Increment</button>
            </div>
        );
    }
    formatCount(){
        const { count } = this.state;
        return count ===0 ? "Zero" : count;
    }

    getBadgeClasses(){ //set className dynamically
		let classes="badge m-2 badge-";
        classes += this.state.count === 0 ?"warning":"primary";
        return classes;
    }
	handleIncrement(product){...;}
}
```

