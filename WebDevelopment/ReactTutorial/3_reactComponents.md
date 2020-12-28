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
+ 

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

### States

+ `this.setState({ username: 'rstacruz' })`