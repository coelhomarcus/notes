# Typescript

> TypeScript é um superset do JavaScript que adiciona tipagem estática, oferecendo melhor IntelliSense, detecção de erros em tempo de compilação e código mais robusto.

## Instalação e Configuração

Configuração inicial do ambiente TypeScript para desenvolvimento.

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

## `Type Annotations`

Anotações de tipo explícitas que definem qual tipo de dado uma variável, parâmetro ou função deve aceitar. Na maioria dos casos, o TypeScript infere os tipos automaticamente.

```ts
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

## `Union Types`

Permite que uma variável aceite múltiplos tipos usando o operador `|`. Útil quando um valor pode ser de diferentes tipos em diferentes situações.

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

## `Operador Opcional`

O operador de encadeamento opcional (`?.`) permite acessar propriedades de objetos que podem ser `null` ou `undefined` sem causar erros.

```ts
const button = document.querySelector("button");
//Apenas executa se for diferente de NULL
button?.click();
```

## `Type`

Types permitem criar aliases para tipos complexos, facilitando reutilização e legibilidade do código. São mais flexíveis que interfaces para union types e primitivos.

```ts
function example(data: { name: string; year: number }) {
  console.log(data.name);
  console.log(data.year);
}
```

Agora utilizando `Type`:

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

## `Interface`

`Interfaces` definem a estrutura de objetos e são extensíveis, sendo ideais para contratos de API e estruturas complexas.

### Básico

Definição simples de interface para tipagem de objetos com propriedades obrigatórias.

```ts
interface User {
  name: string;
  age: number;
  email: string;
}

const user: User = {
  name: "Marcus",
  age: 25,
  email: "marcus@email.com"
};
```

### Propriedades Opcionais

Use `?` para tornar propriedades opcionais, permitindo objetos que não possuem todas as propriedades definidas.

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  description?: string; // Opcional
  inStock?: boolean;    // Opcional
}

const product: Product = {
  id: 1,
  name: "Notebook",
  price: 2500
  // description e inStock são opcionais
};
```

### Propriedades Readonly

Propriedades `readonly` não podem ser modificadas após a inicialização do objeto, garantindo imutabilidade de valores críticos.

```ts
interface Config {
  readonly apiUrl: string;
  readonly version: string;
  timeout: number;
}

const config: Config = {
  apiUrl: "https://api.example.com",
  version: "1.0.0",
  timeout: 5000
};

config.apiUrl = "nova-url"; // ❌ Erro! Propriedade readonly
config.timeout = 3000; // ✅ OK
```

### Extensão de Interfaces

Interfaces podem herdar propriedades de outras interfaces usando `extends`, promovendo reutilização e hierarquia de tipos.

```ts
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  breed: string;
  bark(): void;
}

const dog: Dog = {
  name: "Rex",
  age: 3,
  breed: "Labrador",
  bark() {
    console.log("Woof!");
  }
};
```

### Interfaces com Métodos

Interfaces podem definir assinaturas de métodos, criando contratos para objetos que implementam comportamentos específicos.

```ts
interface Calculator {
  add(a: number, b: number): number;
  subtract(a: number, b: number): number;
}

const calc: Calculator = {
  add(a, b) {
    return a + b;
  },
  subtract(a, b) {
    return a - b;
  }
};
```

### Interface com API

Exemplo prático de interface para tipar dados recebidos de APIs, garantindo estrutura consistente dos dados externos.

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

### Interface vs Type

Comparação entre interfaces (extensíveis, com merge automático) e types (mais flexíveis para unions e primitivos). Escolha baseada no caso de uso.

```ts
// ✅ Interface - Extensível
interface Person {
  name: string;
}

interface Person {
  age: number; // Merge automático
}

// ✅ Type - Mais flexível
type Status = "active" | "inactive";
type ApiResponse<T> = {
  data: T;
  status: Status;
};
```

## `Array`

Tipagem de arrays permite definir que tipo de elementos o array pode conter, oferecendo verificação de tipos e IntelliSense para métodos específicos.

```ts
const numbers = [10, 30, 40, 5, 3, 30];

function maiorQue10(data: number[]) {
  return data.filter((n) => n > 10);
}

maiorQue10(numbers);
```

```ts
const arr = [10, "Coelho", 40, 5, "Marcus", 30];

function onlyNumber(data: (string | number)[]) {
  return data.filter((n) => typeof n === "number");
}

onlyNumber(arr);
```

## Array - Sintaxe Alternativa

Sintaxe genérica para tipagem de arrays usando `Array<Type>`. Funcionalmente equivalente à sintaxe `Type[]`, mas útil para tipos mais complexos.

```ts
const numbers = [10, 30, 40, 5, 3, 30];

function greaterThan10(data: Array<number>) {
  return data.filter((n) => n > 10);
}
```

## Any

O tipo `any` desabilita a verificação de tipos, permitindo qualquer valor. Use com parcimônia pois elimina os benefícios do TypeScript. Prefira `unknown` quando possível.

```ts
function normalize(text: any) {
  return text.trim().toLowerCase();
}

normalize(" DeSIGN");
normalize(200); //erro no runtime
```
