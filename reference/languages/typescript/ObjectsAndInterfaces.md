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
