
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
