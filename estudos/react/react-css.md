# React CSS

### Jeito padrão - importando o .css

```jsx
import "./App.css";

//className="container" -> a classe container esta dentro do App.css que importamos
const App = () => {
  return (
    <div className="container">
      <p className="text">Meu site</p>
    </div>
  );
};
```

### CSS Modules -> aqui as classes são em camelCase e são unicas

```jsx
import styles from "./Produto.module.css";

//styles.title é a classe .title no Produto.module.css

const App = () => {
  return (
    <div>
      <h1 className={styles.title}>Notebook</h1>
      <h2 className={styles.price}>12.000</h2>
    </div>
  );
};
```
