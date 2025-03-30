# Revisão

###### Um componente sempre retorna apenas 1 elemento

```jsx
const ButtonBuy = () => {
  return <button>Buy</button>;
};

//or

const App = () => {
  return (
    <>
      <h1>Best button in the world</h1>
      <ButtonBuy />
    </>
  );
};

// <></> means <React.Fragment></React.Fragment>
```

###### As tags html no JSX tem os atributos em camelCase

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

###### Expressões e Variaveis

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
  const movies = ["Before Sunrise", "Before Sunset", "Before Midnight"];

  return (
    <ul>
      {movies.map((movie) => (
        <li key={movie}>{movie}</li>
      ))}
    </ul>
  );
};
```

###### Componentes

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

###### Um componente pode receber um props

```jsx
import Button from "./Button/Button";

const App = () => {
  return (
    <Button myProp="Hello" />
    //<button>Hello</button>
  );
};

//Button.jsx com desestruturação
const Button = ({ myProp }) => {
  return <button>{myProp}</button>;
};

//Button.jsx sem desestruturação
const Button = (props) => {
  return <button>{props.myProp}</button>;
};
```

###### {children} do Componente

```jsx
import Button from "./Button/Button";

const App = () => {
  return (
    <Button>My Button</Button>
    //"My Button" é fo {children} do button
  );
};

const Button = (props) => {
  return <button>{props.children}</button>;
};
```
