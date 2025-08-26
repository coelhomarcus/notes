# Quick Revision

### Install

```bash
npm install -g typescript
tsc --init
tsc -w
```

```json
//tsconfig.json
{
  "compilerOptions": {
    "target": "ESNext",
    "strict": true
  }
}
```

### Type Annotations

```ts
//Isso não é muito usado pq o tipo ja é declarado na inicialização automaticamente
var username: string = "Marcus";
username = 2; //error
username = "Leda"; //✅

function sum(a: number, b: number) {
  return a + b;
}

function sum(a: number, b: number): number {
  return a + b;
}
```

```ts
const steed = {
  name: "Steed",
  price: 2000,
};

function transformPrice(product: { name: string; price: number }): string {
  return "R$ " + product.price;
}

console.log(transformPrice(steed));
//R$ 2000
```

### Union Types

```ts
let total: string | number = 200;
total = "Hello"; //✅
```

```ts
function isNumber(value: number | number) {
  if (typeof value === "number") {
    return true;
  } else {
    return false;
  }
}
```

### Operador Opcional

```ts
const button = document.querySelector("button");
//Apenas executa se o button for diferente de NULL
button?.click();
```

### Type

```ts
function example(data: { name: string; year: number }) {
  console.log(data.name);
  console.log(data.year);
}
```

```ts
type Data = {
  name: string;
  year: number;
};

function example(data: Data) {
  console.log(data.name);
  console.log(data.year);
}
```

```ts
//Outro exemplo
type numberOrString = number | string;
```

```ts
// Union Types
type Status = "ativo" | "inativo" | "pendente";

// Tupla
type Venda = [string, number, boolean];

// Interseção de tipos
type Pessoa = { nome: string };
type Usuario = Pessoa & { email: string };
```

### Interface

```ts
//Mais usados em objetos
interface Data {
  name: string;
  year: number;
}
```

### Array

```ts
const numbers = [10, 30, 40, 5, 3, 30];

function greaterThan10(data: number[]) {
  return data.filter((n) => n > 10);
}

greaterThan10(numbers);
```

```ts
const arr = [10, "Coelho", 40, 5, "Marcus", 30];

function onlyNumber(data: (string | number)[]) {
  return data.filter((n) => typeof n === "number");
}

onlyNumber(arr);
```

```ts
interface Curso {
  nome: string;
  horas: number;
  aulas: string;
  gratuito: boolean;
  tags: string[];
  idAulas: number[];
  nivel: "iniciante" | "avancado";
}

async function fetchCursos() {
  const response = await fetch("https://api.origamid.dev/json/cursos.json");
  const data = await response.json();
  mostrarCursos(data);
}
```

### Array - Sintaxe Alternativa

```ts
const numbers = [10, 30, 40, 5, 3, 30];

function greaterThan10(data: Array<number>) {
  return data.filter((n) => n > 10);
}
```

### Any

```ts
function normalize(text: any) {
  return text.trim().toLowerCase();
}

normalize(" DeSIGN");
normalize(200); //erro no runtime
```
