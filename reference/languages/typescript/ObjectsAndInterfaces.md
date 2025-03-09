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
