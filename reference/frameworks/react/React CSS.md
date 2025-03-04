# React CSS

### **Default Method** - import through JavaScript
```jsx
import './App.css';

//className="container" -> container is inside of App.css
const App = () => {
  return (
    <div className="container">
      <p className="text">Meu site</p>
    </div>
  );
};
```

### **CSS Modules** - (camelCase in class names)
```jsx
import styles from './Produto.module.css';

//styles.title == class -> .title in Produto.module.css

const App = () => {
  return (
    <div>
      <h1 className={styles.title}>Notebook</h1>
      <h2 className={styles.price}>12.000</h2>
    </div>
  );
};
```
