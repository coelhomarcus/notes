# Quick Tips 🚀
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
//It is not used that much because we already know the type in the declaration.
var username: string = "Marcus"
username = 2; //error
username = "Leda"; //✅

//Inference is mostly used in functions.
function sum(a: number, b: number) {
   return a + b;
}
//We can infer the return type of the function as well
function sum(a: number, b: number): number {
   return a + b;
}
```
```ts
//Example
const steed = {
   name: "Steed",
   price: 2000
}

function transformPrice(product: { name: string, price: number }): string {
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
function isNumber(value: number | number){
   if(typeof value === 'number'){
      return true;
   }else{
      return false;
   }
}
```

### Optional Chaining Operator
```ts
const button = document.querySelector('button');
//Only execute when button is different from null.
button?.click();
```

### Type
```ts
function example(data: {
   name: string,
   year: number
 }) {
   console.log(data.name);
   console.log(data.year);
 }
```
```ts
//using type
type Data = {
   name: string;
   year: number;
}

function example(data: Data) {
   console.log(data.name);
   console.log(data.year);
}
```
```ts
//another example
type numberOrString = number | string;
```

### Interface
```ts
//most used for objects
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