# Objects & Interfaces

### Recapping: class with TypeScript
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
const book = new Product('Harry Potter', 200);

console.log(book instanceof Array); //false
console.log(book instanceof Product); //true
```

### instanceof
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

const game = new Product('Dark Souls', 'Miyazaki');
console.log(game instanceof Game); //true
console.log(game instanceof Product); //true
```

```ts
const link = document.getElementById("coelhomarcus");

if (link instanceof HTMLAnchorElement) {
   link.href = link.href.replace('http://', 'https://');
}
```

### Events & Callbacks
```ts
const button = document.querySelector('button');

function handleClick(event: PointerEvent) {
  console.log(event.pageX);
}

button?.addEventListener('pointerdown', handleClick);
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

document.documentElement.addEventListener('mousedown', activeMenu);
document.documentElement.addEventListener('touchstart', activeMenu);
document.documentElement.addEventListener('pointerdown', activeMenu);
```

### this (in strict mode)
```ts
function activeMenu(this: HTMLButtonElement, event: MouseEvent) {
  console.log(this.innerText);
}

const button = document.querySelector('button');
button?.addEventListener('click', activeMenu);
```

### target & currentTarget
```ts
function activeMenu(event: Event) {
  const element = event.currentTarget;
  if (element instanceof HTMLElement) element.style.background = 'red';
}

const button = document.querySelector('button');
button?.addEventListener('click', activeMenu);

window.addEventListener('keydown', activeMenu);
```

### Generic
```ts
//defining that TIPO is a generic parameter
//for example, if you put <number> it will give an error
function retorno<Tipo>(a: Tipo): Tipo {
  return a;
}

retorno('A Game').charAt(0);
// retorno<string>(a: string): string

retorno(200).toFixed();
// retorno<number>(a: number): number
```

### Generic Extends
```ts
function extractText<Tipo extends HTMLElement>(el: Tipo): string {
  return el.innerText;
}

const link = document.querySelector('a');
```

### Functions
#### Void
```ts
//void to a function that return nothing
function noReturn(color: string): void{
   document.body.style.background = color;
}
```

#### Never
```ts
//Abort the code when an error occurs, and stop running.
function abort(msg: string): never {
   throw new Error(msg);
}

abort("Error");
console.log("dont run");
```

#### w/ Interface
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
   }
}

console.log(calc(squareExample)); //4
```

#### Overload
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

### TypeGuard, Safety e Narrowing

```ts
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function handleData(data: unknown) {
  if (isString(data)) {
    data.toUpperCase();
  }
}

handleData('Origamid');
handleData(200);
```

```ts
async function fetchProduto() {
  const response = await fetch('https://api.origamid.dev/json/notebook.json');
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
    typeof value === 'object' &&
    'nome' in value &&
    'preco' in value
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


#### Unknown
```ts
function test(data: unknown);
```

#### Type Assertion
##### as
```ts
const video = document.querySelector('.player') as HTMLVideoElement;
// erro runtime, não existe volume de null
video.volume;

// erro TS, possíveis dados devem ser compatíveis com Element | null
const link = document.querySelector('.link') as string;
```
##### as with function that return any
```ts
interface Produto {
  nome: string;
  preco: number;
}

async function fetchProduto() {
  const response = await fetch('https://api.origamid.dev/json/notebook.json');
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
  const response = await fetch('https://api.origamid.dev/json/notebook.json');
  return response.json();
}

async function handleProduto2() {
  const produto = (await fetchProduto()) as Produto;
  produto.nome;
}
```

### Destructuring

```ts
const { body }: { body: HTMLElement } = document;

function handleData({ nome, preco }: { nome: string; preco: number }) {
  nome.includes('book');
  preco.toFixed();
}

handleData({
  nome: 'Notebook',
  preco: 2000,
});

```

### Rest

```ts
function comparar(tipo: 'maior' | 'menor', ...numeros: number[]) {
  if (tipo === 'maior') {
    return Math.max(...numeros);
  }
  if (tipo === 'menor') {
    return Math.min(...numeros);
  }
}

console.log(comparar('maior', 3, 2, 4, 30, 5, 6, 20));
console.log(comparar('menor', 3, 2, 4, 1, 5, 6, 20));
```

#### Intersection

```ts
type Produto = {
  preco: number;
};

type Carro = {
  rodas: number;
  portas: number;
};

function handleProdutoCarro(dados: Carro & Produto) {
  dados.rodas;
  dados.portas;
  dados.preco;
}

handleProdutoCarro({
  preco: 20000,
  rodas: 4,
  portas: 5,
});
```

### Others: keyof, class, tuples.
