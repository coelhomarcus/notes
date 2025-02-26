
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
Don't use hooks inside functions, loops, or conditionals.
Hooks should only be used inside functions in custom hooks.
### **useState**
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

### **useEffect**
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
### **useRef** 
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

### **useContext**
useContext will be used to send variables, functions, and other data to child components.
In this example, the variable data (useState) is sent from GlobalContext to the child (Product).
```jsx
//App.jsx
import React from 'react';
import Product from './Product';
import { GlobalStorage } from './GlobalContext';

const App = () => {
	return (
	    <GlobalStorage>
	      <Product />
	    </GlobalStorage>
	  );
};

export default App;
```
```jsx
//GlobalStorage.jsx
import React from 'react';

export const GlobalContext = React.createContext();

export const GlobalStorage = ({ children }) => {

    const [data, setData] = React.useState(null);

    return (
        <GlobalContext.Provider value={{ data }}>
            {children}
        </GlobalContext.Provider>
    );

};
```
```jsx
//Product.jsx
import React from 'react'
import { GlobalContext } from './GlobalContext'

const Product = () => {
    const global = React.useContext(GlobalContext);
    return <div>{global.data}</div>;
}

  
export default Product;
```

### **Custom Hooks**
A custom hook always starts with 'use'. Example: `useCustomHook.jsx`.
```jsx
import React from 'react';

const useFetch = () => {
  const [data, setData] = React.useState(null);
  const [error, setError] = React.useState(null);
  const [loading, setLoading] = React.useState(null);

  const request = React.useCallback(async (url, options) => {
    let response;
    let json;
    try {
      setError(null);
      setLoading(true);
      response = await fetch(url, options);
      json = await response.json();
      if (response.ok === false) throw new Error(json.message);
    } catch (err) {
      json = null;
      setError(err.message);
    } finally {
      setData(json);
      setLoading(false);
      return { response, json };
    }
  }, []);

  return { data, loading, error, request };
};

export default useFetch;
```
```jsx
//App.jsx
import React from 'react';
import useFetch from './useFetch';

const App = () => {
  const { data, loading, error, request } = useFetch();

  React.useEffect(() => {
    request('https://ranekapi.origamid.dev/json/api/produto/notebook');
  }, [request]);

  if (error) return <p>{error}</p>;
  if (loading) return <p>Carregando...</p>;
  if (data) return <div>{data.nome}</div>;
  else return null;
};

export default App;
```

### **Other React Rooks**
- **useMemo**
- **useCallback (important for Custom Hooks)**