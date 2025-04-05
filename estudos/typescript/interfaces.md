# Exemplods de Interfaces 🔎

### Exemplo 1

```ts
interface Empresa {
  nome: string;
  fundacao: number;
  pais: string;
}

interface Product {
  nome: string;
  preco: number;
  descricao: string;
  garantia: string;
  seguroAcidentes: boolean;
  empresaFabricante: Empresa; //Interface EMPRESA
  empresaMontadora: Empresa; //Interface EMPRESA
}

async function fetchProduct() {
  const response = await fetch("https://api.origamid.dev/json/notebook.json");
  const data = await response.json();
  showProduct(data);
}

fetchProduct();

function showProduct(data: Product) {
  console.log(data.nome);
}
```

### Exemplo 2 - Parametros Opcionais

```js
//Parametro Opcional
interface Product {
  name?: string;
}

const game: Product {
   name: "Ragnarok"
}

console.log(game.name?.toLowerCase());
```
