---
Interview graded: true
Last edited time: 2023-08-21T09:36
Needs Rework: false
Status: Not started
---
### Creating react project
1. Install  `npm install -g create-react-app`
2. Execute  `npx create-react-app my-app
	1. You can find a list of available templates by searching for ["cra-template-*"](https://www.npmjs.com/search?q=cra-template-*) on npm.
	2. `--template typescript`
3. `npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev`
4. Edit `.tsx` files for React components and `.ts` files for non-UI logic.

**Or using Vite**


### JSX

	JavaScript extension allowing to use XML. Allows to use only Single Parent. It allows to use variables in html

Some of names are changed

- class → className

```JavaScript
function App() {
	 const name = 'StarGazers'
	return (
		<div className="container">
			<hgroup>
					<h1>React Page <i style={{color: "SteelBlue"}}>{name}<i/></h1>
					<p> React Page text </p>
					<button className="outline" onClick={()=>alert('Hi!')}> Click </button>
			</hgroup>
		</div>
	)
}
export default App;
```

Create conditions

```JavaScript
<button className="outline" onClick={()=>setCount(count + 1)}>
	{(() => {
		if (count === 0)
			return "Click to support";
		else
			return 'Supported ${count} times';
	})()
</button>
```

### Components

Components are the building blocks of any React application, and a single app usually consists of multiple components. A component is essentially a piece of the user interface. It splits the user interface into independent, reusable parts that can be processed separately.

- Reusable code
- Accept props
- Return display

**Class Components**

- Object syntax
- Still valid
- Harder to write

```JavaScript
import React, { Component } from "react";

class Welcome extends React.Component {
	constructor(props){
		super(props);
	}
	render () {
		return <h1>Meet the {this.props.name} </h1>;
	}
}

//usage
<Welcome name="Test"/>
```

**Functions Components**

Special functions that allows you to hook to react components

```JavaScript
const Welcome = (props) => {
	return <h1>Meet the <i>{props.name}</i></h1>
}

//usage
<Welcome name="Test"/>
```

A higher-order component acts as a container for other components. This helps to keep components simple and enables re-usability. They are generally used when multiple components have to use a common logic.

  

**Props**

- [Props](https://www.simplilearn.com/tutorials/reactjs-tutorial/react-props) are short for Properties. It is a React built-in object that stores the value of attributes of a tag and works similarly to HTML attributes.
- Props provide a way to pass data from one component to another component. Props are passed to the component in the same way as arguments are passed in a function.

![[Untitled 59.png|Untitled 59.png]]

|   |   |   |
|---|---|---|
||**State**|**Props**|
|Use|Holds information about the components|Allows to pass data from one component to other components as an argument|
|Mutability|Is mutable|Are immutable|
|Read-Only|Can be changed|Are read-only|
|Child components|Child components cannot access|Child component can access|
|Stateless components|Cannot have state|Can have props|

  

### **Hooks**

**useState**

Creates a variable and it’s setter with initial value

```JavaScript
const [count, setCount] = useState(0) //0 - initial value
```

**useEffect**

Allows you to perform side effects in your components. Some examples of side effects are: fetching data, directly updating the DOM, and timers.

```JavaScript
const pageTitle = document.title;

useEffect ( () => {
	if (count > 0) {
		document.title = '${pageTitle}--${count}
	}
})

useEffect(() => {
  //Runs on every render
});

useEffect(() => {
  //Runs only on the first render
}, []);

useEffect(() => {
  //Runs on the first render
  //And any time any dependency value changes
}, [prop, state]);
```

Some effects require cleanup to reduce memory leaks. Timeouts, subscriptions, event listeners, and other effects that are no longer needed should be disposed. We do this by including a return function at the end of the `useEffect` Hook.

```JavaScript
useEffect(() => {
    let timer = setTimeout(() => {
    setCount((count) => count + 1);
  }, 1000);

  return () => clearTimeout(timer)
  }, []);
```

In short, **useEffect is a tool that lets us interact with the outside world but not affect the rendering or performance of the component that it's in.** In React, we perform API requests within the `useEffect()` hook. It either renders immediately when the app mounts or after a specific state is reached. This is the general syntax we'll use:

```JavaScript
export default () => {
	const [cast, setCast] = useState([]);

	async function fetchCast () {
		const response = await fetch ( 'cast. json');
		setCast(await response.json() );
	}

	useEffect ( () => {fetchCast);});
	return (
		<div style={{
						display: "grid",
						gridTemplateColumns: 'repeat(auto-fit, minmax (90px, 1fr))',
						gap: '1rem',
						marginBottom: '1rem'
		}}>
		{cast.map((member)=>(
				<a key={member.id}>
					<img src={'img/${member.slug}.img'} alt='${member.name}'/>
				</a>
			))}
		</div>
	)
}
```

Custom events passing

```JavaScript
export default () =>{
	const [member, setMember] = useState(null);

	return(
		<ListCast onChoice={ (info) => { seMemberInfo(info)}} />
	)
}
export default ( {onChoice}) => {
	return(
		<a onClick={ () => {onChoice(member)}}/>
	)
})



```

### Structure

```JavaScript
\src
 |App.jsx
 |main.jsx
 \components
  |SomeComponent.jsx
  |AnotherComponent.jsx
```

  

# **What are synthetic events in React?**

- Synthetic events combine the response of different browser's native events into one API, ensuring that the events are consistent across different browsers.
- The application is consistent regardless of the browser it is running in. Here, **preventDefault** is a synthetic event.

## Redux

[Redux](https://www.simplilearn.com/tutorials/reactjs-tutorial/react-with-redux) is an open-source, JavaScript library used to manage the application state. React uses Redux to build the user interface. It is a predictable state container for JavaScript applications and is used for the entire application’s state management.

- **Store:** Holds the state of the application.
- **Action:** The source information for the store.
- **Reducer:** Specifies how the application's state changes in response to actions sent to the store.

![[Untitled 1 17.png|Untitled 1 17.png]]