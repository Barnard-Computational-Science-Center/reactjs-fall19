## React Workshop

##### What is React?
- **"A JavaScript library for building user interfaces"**
- Framework for developing UI components

##### Why use React?
- Useful for developing complex, single-page-applications (SPAs)
- Moving away from directly manipulating HTML/DOM nodes with event listeners, etc.
- Component and data driven development

##### Examples?
- Instagram
- Facebook
- Khan Academy
- [Thinking in React Tutorial](https://reactjs.org/docs/thinking-in-react.html)
- [Ghost documentation](https://docs.ghost.org/)

##### Examples
- [Install node.js](https://nodejs.org/en/)
- [Babel Plugin for Sublime](https://packagecontrol.io/packages/Babel)

---

##### Create React App
Open a terminal, `cd` into your working directory, and run:

	npx create-react-app react-workshop-app
	cd react-workshop-app
	npm start

In Sublime, open your project folder:

	File > Open > ...react-workshop-app > open


##### 1. App.js 
~~~~
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
         Hello Barnard
        </header>
      </div>
    );
  }
}
export default App;
~~~~


##### Process
- Simplify your idea
	- Core functionality?
	- Essential components?
	- Twitter --> micro-blog
	- It's a list
- Prototype, iterate, and fail quickly
- The process is the idea


##### 2. App.js 
~~~~
import React, { Component } from "react";

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">Hello Barnard</header>
        <ul>
          <li> call friend </li>
          <li> math homework </li>
        </ul>
      </div>
    );
  }
}
export default App;
~~~~


##### 3. ToDoList.js 
- Copy / Paste `<ul>` in `App.js` into new file, rename to `ToDoList.js`
- Capitalize every word (to match my code)
- Import `{ Component }`

~~~~
import React, { Component } from 'react';

class ToDoList extends Component {
  render() {
    return (
            <ul>
              <li> call friend </li>
              <li> math homework </li>
            </ul>
        );
      }
    }

export default ToDoList;
~~~~

##### 4. App.js 
- Import & render `ToDoList.js`

~~~~
import React, { Component } from "react";
import ToDoList from "./ToDoList";

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">Hello Barnard</header>

        <ToDoList />
      </div>
    );
  }
}

export default App;
~~~~


##### 5. SubmitToDo.js
~~~~
import React, { Component } from "react";

class SubmitToDo extends Component {
  render() {
    return (
      <form>
        <label>
          To-Do:
          <input />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

export default SubmitToDo;
~~~~


##### 6. App.js 
- Import `SubmitToDo.js`
- Change `header` to `h1`

~~~~
import React, { Component } from "react";
import ToDoList from "./ToDoList";
import SubmitToDo from "./SubmitToDo";

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1 className="App-header">My To Do List</h1>

        <SubmitToDo />
        <ToDoList />
      </div>
    );
  }
}

export default App;
~~~~

##### Discussion 1
- But why React? We're still writing HTML.
- Goal? Take input from form (`SubmitToDo.js`) and render it in `ToDoList.js`
- How to structure this?
- Uni-directional data flow / Single-Source-of-Truth (top-down)
- Capture user data as `state` and communicate it via `props`
- ToDoList.js <— App.js —> SubmitToDo.js


##### 7. SubmitToDo.js
- Add `state`
- Check value with `alert` or `console.log`

~~~~
import React, { Component } from "react";

class SubmitToDo extends Component {
  constructor(props) {
    super(props);
    this.state = { value: "" };
    this.handleChange = this.handleChange.bind(this); // Bind your functions
  }

  handleChange(event) {
    this.setState({ value: event.target.value }, alert(event.target.value));
  }
  render() {
    return (
      <form>
        <label>
          To-Do:
          <input onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

export default SubmitToDo;
~~~~


##### Discussion 2
- Send value from `SubmitToDo.js` to `ToDoList.js`
- Should `SubmitToDo.js` directly update `ToDoList.js?`
- Send data to `App.js`
- Create `state` in `App.js` to "hold" the data
- How to update the `state` of App.js? App.js sends function down to SubmitToDo.js that does this.


##### 8. App.js
- `addNewToDoItem` will update `App.js` state with item from `SubmitToDo.js`
- Pass `addNewToDoItem` to `SubmitToDo.js` (passing `props`)
- `alert` to check

~~~~
import React, { Component } from 'react';
import ToDoList from './ToDoList'
import SubmitToDo from './SubmitToDo'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = { ToDoItems: null };
    // Bind your functions
    this.addNewToDoItem = this.addNewToDoItem.bind(this); 
  }

  addNewToDoItem(item) {
    this.setState({ ToDoItems: item });
    alert(this.state.ToDoItems)
  }

  render() {
    return (
      <div className="App">
        <h1 className="App-header">My To Do List</h1>

        <SubmitToDo addNewToDoItem = {this.addNewToDoItem} />
        <ToDoList />
        
      </div>
    );
  }
}

export default App;
~~~~


##### 9. SubmitToDo.js
- Remove `alert` 
- `handleSubmit` (What happens on user submits an item?)

~~~~
import React, { Component } from 'react';

class SubmitToDo extends Component {
	constructor(props) {
		super(props);
		this.state = {value: ''};
		this.handleChange = this.handleChange.bind(this) // Bind your functions
		this.handleSubmit = this.handleSubmit.bind(this)
	}

	handleChange(event) {
		this.setState({value: event.target.value})
	}

	handleSubmit(event) {
		event.preventDefault(); // onSubmit handler default 
		this.props.addNewToDoItem(this.state.value)
	}

    render() {
	    return (
			<form onSubmit = {this.handleSubmit}>
			  <label>
			    To-Do:
			    <input onChange={this.handleChange} />
			  </label>
			  <input type="submit" value="Submit" />
			</form>
	        );
	      }
    }

export default SubmitToDo;
~~~~


##### 10. App.js 
- Pass `ToDoItems` to `ToDoList.js`

~~~~
import React, { Component } from 'react';
import ToDoList from './ToDoList'
import SubmitToDo from './SubmitToDo'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = { ToDoItems: null };
    // Bind your functions
    this.addNewToDoItem = this.addNewToDoItem.bind(this); 
  }

  addNewToDoItem(item) {
    this.setState({ ToDoItems: item });
    alert(this.state.ToDoItems)
  }

  render() {
    return (
      <div className="App">
        <h1 className="App-header">My To Do List</h1>

        <SubmitToDo addNewToDoItem = {this.addNewToDoItem} />
        <ToDoList ToDoItems = {this.state.ToDoItems} />
        
      </div>
    );
  }
}

export default App;
~~~~


##### 11. ToDoList.js 
- Check if `ToDoList.js` component has received the data (`ToDoItems`)

~~~~
import React, { Component } from "react";

class ToDoList extends Component {
  render() {
    const todos = this.props.ToDoItems;
    console.log(todos);
    
    return (
      <ul>
        <li> call friend </li>
        <li> math homework </li>
      </ul>
    );
  }
}

export default ToDoList;
~~~~


##### 12. App.js 
- Change `ToDoItems` to an array
- Update `addNewToDoItem` to push into array

~~~~
import React, { Component } from 'react';
import ToDoList from './ToDoList'
import SubmitToDo from './SubmitToDo'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = { ToDoItems: [] };
    // Bind your functions
    this.addNewToDoItem = this.addNewToDoItem.bind(this); 
  }

  addNewToDoItem(item) {
    let ToDoItems = [...this.state.ToDoItems]
        ToDoItems.push(item)
        this.setState({ToDoItems: ToDoItems})
  }

  render() {
    return (
      <div className="App">
        <h1 className="App-header">My To Do List</h1>

        <SubmitToDo addNewToDoItem = {this.addNewToDoItem} />
        <ToDoList ToDoItems = {this.state.ToDoItems} />
        
      </div>
    );
  }
}

export default App;
~~~~


##### 13. ToDoList.js 
- Rewrite as functional component
- Don't have to call `this`

~~~~
import React, { Component } from "react";

const ToDoList = (props) => {
  const todos = props.ToDoItems;

  return (
  		<ul>
  			{todos}
  		</ul>
  );
};

export default ToDoList;
~~~~


##### 14. ToDoList.js 
- `map` through `ToDoItems` to create `<li>`

~~~~
import React, { Component } from "react";

const ToDoList = (props) => {
  const todos = props.ToDoItems;
  const listItems = todos.map(item => <li>{item}</li>)

  return (
  		<ul>
  			{listItems}
  		</ul>
  );
};

export default ToDoList;
~~~~


##### Styling & Bootstrap
- [React Bootstrap](https://react-bootstrap.github.io/getting-started/introduction)

Quit the terminal (`cmd + c`) and install `react-bootstrap`

	npm install react-bootstrap bootstrap
	npm start

In `App.js`, add the stylesheet:

	import 'bootstrap/dist/css/bootstrap.min.css';


##### 15. ToDoList.js 
- `map` through `ToDoItems` to create `<li>`

~~~~
import React, { Component } from "react";
import Card from "react-bootstrap/Card";
import Container from "react-bootstrap/Container";

const ToDoList = (props) => {
  const todos = props.ToDoItems;
  const listItems = todos.map(item => 
  	<Container className="p-3">
	  	<Card>
	  		<p>{item}</p>
	  	</Card>
  	</Container>
  )

  return (
	  	<div>
	  		{listItems}
	  	</div>
  );
};

export default ToDoList;
~~~~


##### 16. ToDoList.js 
- Using the `ListGroup` component

~~~~
import React, { Component } from "react";
import Card from "react-bootstrap/Card";
import Container from "react-bootstrap/Container";
import ListGroup from "react-bootstrap/ListGroup";

const ToDoList = (props) => {
  const todos = props.ToDoItems;
  const listItems = todos.map(item => 
  	<ListGroup.Item> {item} </ListGroup.Item>
  )

  return (
	<Card style={{ width: '18rem' }}>
	  <ListGroup variant="flush">
	    
	    { listItems }
	    
	  </ListGroup>
	</Card>
  );
};

export default ToDoList;
~~~~


