# React - Fundamentos

> Biblioteca JavaScript para construção de interfaces de usuário baseada em componentes reutilizáveis e estado reativo.

## Estrutura de Componentes

Um componente React sempre retorna apenas `1` elemento ou fragmento, mantendo uma estrutura hierárquica clara e previsível.

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

// <></> significa <React.Fragment></React.Fragment>
```

### JSX - Sintaxe e Atributos

JSX (JavaScript XML) permite escrever HTML dentro do JavaScript. Atributos seguem camelCase e alguns nomes mudam para evitar conflitos com palavras reservadas do JavaScript.

```jsx
const App = () => {
  return (
	  <div className="content">1</div>
	  <h1 style={{color: "black", display: "flex"}}>2</h1>
	  <a href="https://www.com">link</a>
	  <video autoPlay />;
      );
};
```

## `Expressões` e `Variáveis`

JSX permite inserir expressões JavaScript usando chaves `{}`, possibilitando rendering dinâmico de conteúdo baseado em variáveis, funções e operadores.

```jsx
const App = () => {
  const name = "Marcus";
  return <p>{name}</p>;
};
```
Outro exemplo:
```jsx
const active = true;

const App = () => {
  return <p className={active ? "activeClass" : "disableClass"}>text</p>;
};
```

## `Arrays` e Renderização de Listas

Rendering de listas usando `map()` para transformar arrays de dados em elementos JSX. Cada item deve ter uma `key` única para otimização do React.

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

## `Componentes` e Props

Componentes são funções que retornam JSX e podem receber dados através de `props`. Permitem reutilização de código e composição de interfaces complexas.

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

### Props - Passagem de Dados

Props são argumentos passados para componentes, permitindo personalização e comunicação entre componentes pai e filho. São imutáveis dentro do componente.

```jsx
//App.jsx
import Button from "./Button/Button";

const App = () => {
  return (
    <Button myProp="Hello" />
    //<button>Hello</button>
  );
};
```

```jsx
//Button.jsx
const Button = (props) => {
  return <button>{props.myProp}</button>;
};

//Button.jsx com desestruturação
const Button = ({ myProp }) => {
  return <button>{myProp}</button>;
};
```

### `{children}` - Composição de Componentes

A prop especial `children` permite que componentes envolvam outros elementos, criando layouts flexíveis e componentes reutilizáveis como containers.

```jsx
//App.jsx
import Button from "./Button/Button";

const App = () => {
  return (
    <Button>My Button</Button>
    //"My Button" é o {children} do button
  );
};
```
```jsx
//Button.jsx
const Button = (props) => {
  return <button>{props.children}</button>;
};

//Desestruturação com children
const Button = ({ children }) => {
  return <button>{children}</button>;
};
```

## 📚 Navegação

| **Atual** | **Próximo →** |
|---|---|
**React Básico** | [React Hooks](./1-react-hooks.md) |
