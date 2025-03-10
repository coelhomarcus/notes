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