# React Router

> Biblioteca para roteamento client-side em aplicações React, permitindo navegação entre páginas sem recarregar a página.

## Instalação

```bash
npm install react-router
```

### `BrowserRouter`

Componente raiz que habilita o roteamento na aplicação. Deve envolver toda a aplicação para permitir navegação entre rotas.

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

Define as rotas da aplicação. `Routes` agrupa múltiplas `Route`, cada uma associando um caminho (path) a um componente.

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

Substitui as tags `<a>` para navegação interna. Evita o reload da página e mantém o estado da aplicação.

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

Versão especial do `Link` que adiciona automaticamente a classe `active` no link da página atual. Ideal para menus de navegação.

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

Hook para navegação programática (via JavaScript). Útil para redirecionamentos após ações como login, cadastro ou validações.

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

Hook que extrai parâmetros dinâmicos da URL (como IDs). Permite criar rotas dinâmicas para páginas de detalhes.

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

## 📚 Navegação

| **Anterior** | **Atual →** |
|---|---|
[React Hooks](./2-react-css.md) | **React Router**
