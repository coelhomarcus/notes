# React Hooks

Não use hooks dentro de `funções`, `loops` ou `condicionais`.

`Hooks` só devem ser usado usados dentro de `funções` se forem `Custom Hooks`.

### **useState**

Utilizamos o `useState` para informar ao React quando uma variável é alterada, de modo que ele possa re-renderizar o componente e atualizar o estado.

```jsx
const App = () => {
  //React.useState(0)
  //0 é o valor inicial do state count
  const [count, setCount] = React.useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return <button onClick={handleClick}>my button</button>;
};
```

### **useEffect**

Utilizamos o `useEffect` para executar `efeitos colaterais` em um componente, como buscar dados ou reagir a mudanças no estado, sempre que determinadas dependências forem atualizadas.

Por exemplo sempre que o estado do contador (`count`) mudar, ele da um `console.log()`

```jsx
const App = () => {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    console.log("count mudou");
  }, [count]);

  function handleClick() {
    setCount(count + 1);
  }

  return <button onClick={handleClick}>my button</button>;
};
```

Quando passamos um `array vazio` como dependência no `useEffect`, a função é executada apenas uma vez, logo após a primeira renderização do componente.

```jsx
const App = () => {
  React.useEffect(() => {
    console.log("Roda uma vez");
  }, []);

  return <div>hello</div>;
};
```

Por exemplo o `useEffect` é utilizado para criar um `eventListener` no scroll, então assim que o componente for rederizado o scroll tera esse evento adicionado.

```jsx
const App = () => {
  React.useEffect(() => {
    function handleScroll(event) {
      console.log(event);
    }
    window.addEventListener("scroll", handleScroll);
    // Limpa o evento no retorno da função
    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  }, []);

  return <p style={{ height: "200vh" }}>Scroll gigante</p>;
};
```

### **useRef**

Hook para referenciar elementos DOM diretamente ou armazenar valores mutáveis que não causam re-renderização quando alterados.

```jsx
const App = () => {
  //Usado para referenciar um elemento
  const button = React.useRef();

  console.log(button.current);
  //<button ref={button}>meu button</button>

  return <button ref={button}>meu button</button>;
};
```

```jsx
const timeoutRef = React.useRef();

//Tambem usado para salvar uma variavel que não muda entre as renderizações

function handleClick() {
  setNotification("Thanks for buy");

  clearTimeout(timeoutRef.current);

  timeoutRef.current = setTimeout(() => {
    setNotification(null);
  }, 1000);
}
```

### **useContext**

Hook que permite compartilhar dados entre componentes sem prop drilling. Consome valores de um Context Provider.

```jsx
//App.jsx
import React from "react";
import Product from "./Product";
import { GlobalStorage } from "./GlobalContext";

const App = () => {
  return (
    <GlobalStorage>
            <Product />   {" "}
    </GlobalStorage>
  );
};

export default App;
```

```jsx
//GlobalStorage.jsx
import React from "react";

export const GlobalContext = React.createContext();

export const GlobalStorage = ({ children }) => {
  const [data, setData] = React.useState(null);

  return (
    <GlobalContext.Provider value={{ data }}>
                  {children}       {" "}
    </GlobalContext.Provider>
  );
};
```

```jsx
//Product.jsx
import React from "react";
import { GlobalContext } from "./GlobalContext";

const Product = () => {
  const global = React.useContext(GlobalContext);
  return <div>{global.data}</div>;
};

export default Product;
```

### **Custom Hooks**

Funções que encapsulam lógica de hooks reutilizável. Sempre começam com "use" e permitem compartilhar lógica entre componentes.

```jsx
import React from "react";

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
import React from "react";
import useFetch from "./useFetch";

const App = () => {
  const { data, loading, error, request } = useFetch();

  React.useEffect(() => {
    request("https://ranekapi.origamid.dev/json/api/produto/notebook");
  }, [request]);

  if (error) return <p>{error}</p>;
  if (loading) return <p>Carregando...</p>;
  if (data) return <div>{data.nome}</div>;
  else return null;
};

export default App;
```

### **Outros React Hooks**

Hooks adicionais para otimização de performance e casos específicos:

- **`useMemo`** - Memoriza resultados de cálculos complexos para evitar recálculos desnecessários
- **`useCallback`** - Memoriza funções para evitar recriações e otimizar performance de componentes filhos
