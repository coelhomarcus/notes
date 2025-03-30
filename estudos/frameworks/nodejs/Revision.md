## Primeiro codigo no Node JS

Utilizando o modulo node:fs

```js
//Importação
import { writeFile, readFile } from "node:fs";

//Escrevendo algo em um arquivo
//Nome do Arquivo, Conteúdo, Função de Callback
writeFile("file.txt", "Hello World!", (err) => {
  if (err) {
    console.log(err);
  }
});

//Lendo arquivo
//Nome do Arquivo, Tipo de Dado, Função de Callback
readFile("./file.txt", "utf8", (err, data) => {
  console.log(data);
});
```
