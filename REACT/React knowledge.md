
# Quick Tips 🚀
###### A **component** always return a single element
```jsx
const ButtonBuy = () => {
  return <button>Buy</button>;
};

//or

const App = () => {
  return(
	  <>
		  <h1>Best button in the world</h1>
		  <ButtonBuy />
	  </>
  );
};

// <></> means <React.Fragment></React.Fragment>
```

###### **HTML tags** contain attributes, but in **camelCase** in **React**.
```jsx
//Attributes examples
const App = () => {
  return (
	  <div className="content">1</div>
	  <h1 style={{color: "black", display: "flex"}}>2</h1>
	  <a href="https://www.com">link</a>
	  <video autoPlay />;
      );
};
```

###### **Expressions** / **Variables**
```jsx
const App = () => {
	const name = "Marcus";
	return <p>{name}</p>;
};

//Another example

const active = true;

const App = () => {
	return <p className={active ? "activeClass" : "disableClass"}>text</p>;
};

```

###### **Array**
```jsx
const App = () => {
  const movies = ['Before Sunrise', 'Before Sunset', 'Before Midnight'];

  return (
    <ul>
      {movies.map((movie) => (
        <li key={movie}>{movie}</li>
      ))}
    </ul>
  );
};
```


###### **Components**
```jsx
import Header from "./Header/Header"
import Footer from "./Footer/Footer"

const App = () => {
  return (
    <Header />
    <h1>Content</h1>
	<Footer />
  );
};
```

###### **Component** receive props
```jsx
import Button from "./Button/Button"

const App = () => {
  return (
    <Button myProp="Hello" />
    //<button>Hello</button>
  );
};

//Button.jsx with destructuring 
const Button = ({myProp}) => {
  return (
    <button>{myProp}</button>
  );
};

//Button.jsx with no destructuring 
const Button = (props) => {
  return (
    <button>{props.myProp}</button>
  );
};
```

###### **Component** children
```jsx
import Button from "./Button/Button"

const App = () => {
  return (
    <Button>My Button</Button>
    //"My Button" is a child of Button
  );
};

const Button = (props) => {
  return (
    <button>{props.children}</button> 
  );
};

```


# React Hooks
###### **useState**
```jsx
const App = () => {
	//useState(0)
	//0 is the initial value of count
	const [count, setCount] = React.useState(0);
	
	function handleClick(){
		setCount(count + 1);
	}

	return <button onClick={handleClick}>my button</button>;
};
```

###### **useEffect**
```jsx
const App = () => {
	const [count, setCount] = React.useState(0);
	
	React.useEffect(()=>{
		console.log("count changed");
	}, [count]);
	
	function handleClick(){
		setCount(count + 1);
	}

	return <button onClick={handleClick}>my button</button>;
};
```

```jsx
const App = () => {
	React.useEffect(()=>{
		console.log("Runs once");
	}, []);
	
	return <div>hello</div>;
};
```

```jsx
const App = () => {
	React.useEffect(() => {
    function handleScroll(event) {
      console.log(event);
    }
    window.addEventListener('scroll', handleScroll);
    // Clear when the event is removed
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);
	
	return <p style={{ height: '200vh' }}>Giant Page for Scroll xD</p>;
};
```
###### **useRef** 
```jsx
const App = () => {
	//used to reference an element
	const button = React.useRef();

	console.log(button.current);
	//<button ref={button}>my button</button>

	return <button ref={button}>my button</button>;
};
```

```jsx
const timeoutRef = React.useRef();

//also used to save a variable that does not reset on every render

function handleClick() {
    setNotification('Thanks for buy');
    
    clearTimeout(timeoutRef.current);
    
    timeoutRef.current = setTimeout(() => {
      setNotification(null);
    }, 1000);
}
```

