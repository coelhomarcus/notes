# Objetos e Interfaces

> Guia completo sobre tipagem de objetos, classes e interfaces em TypeScript para desenvolvimento web.

## `Classes` em `TypeScript`

Classes em TypeScript permitem definir blueprints para objetos com propriedades tipadas e métodos. Elas oferecem uma estrutura orientada a objetos com verificação de tipos em tempo de compilação.

```ts
class Product {
  name: string;
  price: number;
  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }
  priceBRL() {
    return `R$ ${this.price}`;
  }
}
const book = new Product("Harry Potter", 200);

console.log(book instanceof Array); //false
console.log(book instanceof Product); //true
```

## `instanceof`

O operador `instanceof` verifica se um objeto foi criado a partir de uma classe específica, permitindo type guards e verificações de tipo em tempo de execução.

```ts
console.log(book instanceof Array); //false
console.log(book instanceof Product); //true
```

```ts
class Product {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}

class Game extends Product {
  autor: string;
  constructor(name: string, autor: string) {
    super(name);
    this.autor = autor;
  }
}

const game = new Product("Dark Souls", "Miyazaki");
console.log(game instanceof Game); //true
console.log(game instanceof Product); //true
```

```ts
const link = document.getElementById("coelhomarcus");

if (link instanceof HTMLAnchorElement) {
  link.href = link.href.replace("http://", "https://");
}
```

## `Events & Callbacks`

TypeScript oferece tipagem específica para eventos DOM, permitindo acesso seguro às propriedades de diferentes tipos de eventos (Mouse, Touch, Keyboard, etc.).

```ts
const button = document.querySelector("button");

function handleClick(event: PointerEvent) {
  console.log(event.pageX);
}

button?.addEventListener("pointerdown", handleClick);
```

```ts
function activeMenu(event: Event) {
  console.log(event.type);
  if (event instanceof MouseEvent) {
    console.log(event.pageX);
  }
  if (event instanceof TouchEvent) {
    console.log(event.touches[0].pageX);
  }
}

document.documentElement.addEventListener("mousedown", activeMenu);
document.documentElement.addEventListener("touchstart", activeMenu);
document.documentElement.addEventListener("pointerdown", activeMenu);
```

## this (strict mode)

No modo estrito, você pode tipar explicitamente o contexto `this` de uma função para garantir que ela seja chamada no elemento correto.

```ts
function activeMenu(this: HTMLButtonElement, event: MouseEvent) {
  console.log(this.innerText);
}

const button = document.querySelector("button");
button?.addEventListener("click", activeMenu);
```

## `target & currentTarget`

Diferenciação entre o elemento que disparou o evento (`target`) e o elemento que possui o event listener (`currentTarget`), com verificação de tipos segura.

```ts
function activeMenu(event: Event) {
  const element = event.currentTarget;
  if (element instanceof HTMLElement) element.style.background = "red";
}

const button = document.querySelector("button");
button?.addEventListener("click", activeMenu);

window.addEventListener("keydown", activeMenu);
```

## `Genericos`

Generics permitem criar componentes reutilizáveis que trabalham com diferentes tipos, mantendo a segurança de tipos. São como "parâmetros de tipo" para funções e classes.

```ts
//Defininido que <Tipo> é um Generico
function retorno<Tipo>(a: Tipo): Tipo {
  return a;
}

retorno("A Game").charAt(0);
// retorno<string>(a: string): string

retorno(200).toFixed();
// retorno<number>(a: number): number
```

## `Generic Extends`

Restringe os tipos genéricos a subtipos específicos, garantindo que o tipo genérico tenha certas propriedades ou métodos disponíveis.

```ts
function extractText<Tipo extends HTMLElement>(el: Tipo): string {
  return el.innerText;
}

const link = document.querySelector("a");
```

## `Functions`

Tipagem avançada de funções em TypeScript, incluindo diferentes tipos de retorno e padrões de uso específicos.

### `Void`

```ts
//void por que a função não retorna nada
function noReturn(color: string): void {
  document.body.style.background = color;
}
```

### `Never`

O tipo `never` representa funções que nunca retornam normalmente (sempre lançam erro ou entram em loop infinito).

```ts
//Aborta o codigo se um erro acontecer, e para de rodar.
function abort(msg: string): never {
  throw new Error(msg);
}

abort("Error");
console.log("dont run");
```

### `w/ Interface`

Uso de interfaces para definir contratos de função, garantindo que objetos passados como parâmetros tenham a estrutura esperada.

```js
interface Square {
  side: number;
  perimeter(side: number): number;
}

function calc(shape: Square) {
  return shape.perimeter(shape.side);
}

const squareExample: Square = {
  side: 2,
  perimeter(side) {
    return side * side;
  },
};

console.log(calc(squareExample)); //4
```

### `Overload`

Function overload permite definir múltiplas assinaturas para a mesma função, oferecendo diferentes tipos de entrada e saída baseados nos parâmetros.

```ts
// Exemplo 1
function normalizar(valor: string): string;
function normalizar(valor: string[]): string[];
function normalizar(valor: string | string[]): string | string[] {
  if (typeof valor === "string") {
    return valor.trim().toLowerCase();
  } else {
    return valor.map((item) => item.trim().toLowerCase());
  }
}

normalizar(" Produto ");
normalizar(["Banana ", " UVA"]);
```

## TypeGuard, Safety e Narrowing

Type guards são funções que verificam tipos em tempo de execução, permitindo que o TypeScript "afunile" (narrow) tipos e ofereça melhor IntelliSense e segurança.

```ts
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function handleData(data: unknown) {
  if (isString(data)) {
    data.toUpperCase();
  }
}

handleData("Origamid");
handleData(200);
```

```ts
async function fetchProduto() {
  const response = await fetch("https://api.origamid.dev/json/notebook.json");
  const json = await response.json();
  handleProduto(json);
}
fetchProduto();

interface Produto {
  nome: string;
  preco: number;
}

function isProduto(value: unknown): value is Produto {
  if (
    value &&
    typeof value === "object" &&
    "nome" in value &&
    "preco" in value
  ) {
    return true;
  } else {
    return false;
  }
}

function handleProduto(data: unknown) {
  if (isProduto(data)) {
    console.log(data);
  }
}
```

### `Unknown`

O tipo `unknown` é uma alternativa mais segura ao `any`, exigindo verificação de tipo antes do uso. É o tipo mais restritivo para valores desconhecidos.

```ts
function test(data: unknown);
```

### `Type Assertion`

Type assertion permite "forçar" um tipo quando você tem certeza do tipo real, mas o TypeScript não consegue inferir automaticamente.

#### `as`

```ts
const video = document.querySelector(".player") as HTMLVideoElement;
// erro runtime, não existe volume de null
video.volume;

// erro TS, possíveis dados devem ser compatíveis com Element | null
const link = document.querySelector(".link") as string;
```

#### `as with function that return any`

```ts
interface Produto {
  nome: string;
  preco: number;
}

async function fetchProduto() {
  const response = await fetch("https://api.origamid.dev/json/notebook.json");
  return response.json() as Promise<Produto>;
}

// Podemos usar o as em diferentes locais.
async function handleProduto1() {
  const produto = await fetchProduto();
  produto.nome;
}
```

```ts
async function fetchProduto() {
  const response = await fetch("https://api.origamid.dev/json/notebook.json");
  return response.json();
}

async function handleProduto2() {
  const produto = (await fetchProduto()) as Produto;
  produto.nome;
}
```

## `Destructuring`

Desestruturação com tipagem explícita permite extrair propriedades de objetos mantendo a segurança de tipos do TypeScript.

```ts
const { body }: { body: HTMLElement } = document;

function handleData({ nome, preco }: { nome: string; preco: number }) {
  nome.includes("book");
  preco.toFixed();
}

handleData({
  nome: "Notebook",
  preco: 2000,
});
```

## `Rest`

O operador rest (`...`) permite capturar múltiplos argumentos em um array tipado, útil para funções com número variável de parâmetros.

```ts
function comparar(tipo: "maior" | "menor", ...numeros: number[]) {
  if (tipo === "maior") {
    return Math.max(...numeros);
  }
  if (tipo === "menor") {
    return Math.min(...numeros);
  }
}

console.log(comparar("maior", 3, 2, 4, 30, 5, 6, 20));
console.log(comparar("menor", 3, 2, 4, 1, 5, 6, 20));
```

### `Intersection`

Intersection types (`&`) combinam múltiplos tipos em um só, criando um tipo que possui todas as propriedades dos tipos combinados.

```ts
// Definindo tipos separados para diferentes categorias
type Produto = {
  nome: string;
  preco: number;
  categoria: string;
};

type Veiculo = {
  marca: string;
  ano: number;
  rodas: number;
};

// Intersection: CarroProduto possui TODAS as propriedades de ambos os tipos
type CarroProduto = Produto & Veiculo;

// Função que aceita um objeto com propriedades de ambos os tipos
function processarCarroParaVenda(item: CarroProduto) {
  // Propriedades de Produto
  console.log(`Produto: ${item.nome}`);
  console.log(`Preço: R$ ${item.preco.toLocaleString()}`);
  console.log(`Categoria: ${item.categoria}`);

  // Propriedades de Veiculo
  console.log(`Marca: ${item.marca}`);
  console.log(`Ano: ${item.ano}`);
  console.log(`Rodas: ${item.rodas}`);

  return `${item.marca} ${item.nome} - ${item.ano} por R$ ${item.preco}`;
}

// Objeto deve ter TODAS as propriedades dos dois tipos
const carroUsado: CarroProduto = {
  // Propriedades de Produto
  nome: "Civic",
  preco: 85000,
  categoria: "Sedan",

  // Propriedades de Veiculo
  marca: "Honda",
  ano: 2020,
  rodas: 4
};

processarCarroParaVenda(carroUsado);
// Output: Honda Civic - 2020 por R$ 85000
```

**Exemplo prático com múltiplas intersections:**

```ts
type Identificavel = {
  id: string;
  criadoEm: Date;
};

type Editavel = {
  editadoEm?: Date;
  editadoPor?: string;
};

type Usuario = {
  nome: string;
  email: string;
};

// Combinando múltiplos tipos
type UsuarioCompleto = Usuario & Identificavel & Editavel;

const usuario: UsuarioCompleto = {
  id: "usr_123",
  criadoEm: new Date(),
  nome: "Marcus",
  email: "marcus@email.com",
  editadoEm: new Date(),
  editadoPor: "admin"
};
```

## Others: keyof, class, tuples.

---

## 📚 Navegação

### 📖 Sequência de Estudo TypeScript

| **← Anterior** | **Atual** |
|---|---|
| [📘 TypeScript Básico](./0-typescript.md) | **🏗️ Objects & Interfaces** |
