# [React Router](https://reactrouter.com/)

```bash
npm install react-router
```

### **main.jsx - BrowserRouter**
```jsx
import React from "react";
import { BrowserRouter } from "react-router";
import App from "./app";

const root = document.getElementById("root");

//here
ReactDOM.createRoot(root).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

### **App.jsx - Routes, Route**
```jsx
import React from "react";
import { Routes, Route } from 'react-router';

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

### **Link**
```jsx
import React from "react";
import { Routes, Route } from 'react-router';

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

### **NavLink**
```jsx
import React from "react";
import { Routes, Route } from 'react-router';

//The difference is in the NavLink: the active element receives the "active" class.
const Header = () => {
  return (
    <nav>
      <NavLink to="/" end activeStyle={activeStyle}>Home </NavLink>
      <NavLink to="about" activeStyle={activeStyle}>About</NavLink>
      <NavLink to="contact" activeStyle={activeStyle}>Contact</NavLink>
    </nav>
  );
};
```

### **useNavigate**
```jsx
import { useNavigate } from 'react-router';

const Login = () => {
  const navigate = useNavigate();

  function handleClick() {
    console.log('Faz o login');
    navigate('/about');
  }

  return (
    <div>
      <button onClick={handleClick}>Login</button>
    </div>
  );
};
```

### **useParams**
```jsx
import { Routes, Route } from 'react-router';

const App = () => {
  return (
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="produto/:id" element={<Produto />} /> //parameter id <-
      </Routes>
  );
};

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

### other things: **useLocation, Nested Routes**

#### Extra
```jsx
//change the html head.
const Head = (props) => {
  React.useEffect(() => {
    document.title = props.title;
    document
      .querySelector("meta[name='description']")
      .setAttribute('content', props.description);
  }, [props]);

  return <></>;
};
```
