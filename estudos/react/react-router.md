# React Router

Instalação:

```bash
npm install react-router
```

### `BrowserRouter`

```jsx
//main.jsx
import React from "react";
import { BrowserRouter } from "react-router";
import App from "./app";

const root = document.getElementById("root");

//BrowserRouter usado aqui
ReactDOM.createRoot(root).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

### `Routes & Route`

```jsx
//App.jsx
import React from "react";
import { Routes, Route } from "react-router";

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="sobre" element={<About />} />
      <Route path="contato" element={<Contact />} />
      <Route path="*" element={<Page404 />} />
    </Routes>
  );
};
```

### `Link`

```jsx
import React from "react";
import { Routes, Route } from "react-router";

const Header = () => {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="sobre">Sobre</Link>
      <Link to="contato">Contato</Link>
    </nav>
  );
};
```

### `NavLink`

A diferença entre `link` e `navlink` é que o `navlink` adiciona a `classe active` no que esta selecionado no momento.

```jsx
import React from "react";
import { Routes, Route } from "react-router";

const Header = () => {
  return (
    <nav>
      <NavLink to="/" end activeStyle={activeStyle}>
        Home{" "}
      </NavLink>
      <NavLink to="about" activeStyle={activeStyle}>
        About
      </NavLink>
      <NavLink to="contact" activeStyle={activeStyle}>
        Contact
      </NavLink>
    </nav>
  );
};
```

### `useNavigate`

O `useNavigate` é um hook do React Router usado para navegar programaticamente entre rotas.

Nesse exemplo, simulamos um login e o usuário seria redirecionado para a página `/about`.

```jsx
import { useNavigate } from "react-router";

const Login = () => {
  const navigate = useNavigate();

  function handleClick() {
    console.log("Faz o login");
    navigate("/about");
  }

  return (
    <div>
      <button onClick={handleClick}>Login</button>
    </div>
  );
};
```

### `useParams`

```jsx
//App.jsx
import { Routes, Route } from 'react-router';

const App = () => {
  return (
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="produto/:id" element={<Produto />} />
      </Routes>
  );
};
```

```jsx
//Product.jsx
import { useParams } from 'react-router';

const Product = () => {
  const params = useParams();
  return (
    <div>
      <h1>Produto</h1>
      <p>id: {params.id}</p>
    </div>
  );
};
```

Outros Conceitos: `useLocation`, `Nested Routes`

Documentação: https://reactrouter.com/
