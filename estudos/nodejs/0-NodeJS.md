## Codigo em `Node.js`

Utilizando o modulo `node:fs`, biblioteca padrão do `Node.js`.

```js
// Importação
import { writeFile, readFile } from "node:fs";

// Escrevendo em um arquivo
// Nome do Arquivo, Conteúdo, Função Callback
writeFile("file.txt", "Hello World!", (err) => {
  if (err) {
    console.log(err);
  }
});

// Lendo arquivo
// Nome do Arquivo, Encode, Função Callback
readFile("./file.txt", "utf8", (err, data) => {
  console.log(data);
});
```

> Talvez eu atualize no futuro, mas aqui é basicamente JavaScript `:P`
