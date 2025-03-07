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