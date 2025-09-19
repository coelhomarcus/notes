# Estilização em React

### Importando o `.css` padrão

Importamos normalmente `import "./App.css";` e assim ja conseguimos acessar todas as classes dentro de `App.css` por exemplo a classe `container`.

```jsx
import "./App.css";

const App = () => {
  return (
    <div className="container">
      <p>Meu site</p>
    </div>
  );
};
```

### CSS Modules

Em `CSS Modules` as `classes` são em `camelCase` e unicas.

No exemplo abaixo importamos os estilos de `Produto.module.css` que tem duas classes `.title` e `.price`, para acessa-las usamos o `styles` que importamos e acessamos as classes que agora viram propriedades `styles.title` e `styles.price`

```jsx
import styles from "./Produto.module.css";

const App = () => {
  return (
    <div>
      <h1 className={styles.title}>Notebook</h1>
      <h2 className={styles.price}>12.000</h2>
    </div>
  );
};
```

## 📚 Navegação

| **← Anterior** | **Atual** | **Próximo →** |
|---|---|---|
| [React Hooks](./1-react-hooks.md) | **React Estilização** | [React Router](./3-react-router.md) |
