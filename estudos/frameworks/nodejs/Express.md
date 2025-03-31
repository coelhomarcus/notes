### Express

Apesar de ser um framework vou deixar aqui.

### Instalação

```bash
npm init -y

npm install express
```

```json
//mudar o arquivo main em package.json para index.js ou o nome que estiver
"main": "index.js"
//tambem mudar o tipo
"type": "module"
```

### NodeMon

vamos usar para executar e nao precisar ficar fechando e rodando dnv com alterações `nodemon index.js`

```bash
npm install -g nodemon
```

### Criando Aplicação

```js
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send(`<h1>Olá Get</h1>`);
});

app.post("/post", (req, res) => {
  res.send(`<h1>Olá Post</h1>`);
  res.sendStatus(201);
});

app.put("/put", (req, res) => {
  res.send(`<h1>Olá Put</h1>`);
  res.sendStatus(200);
});

app.delete("/delete", (req, res) => {
  res.send(`<h1>Olá Delete</h1>`);
  res.sendStatus(200);
});

app.listen(port, () => {
  console.log(`Servidor rodando na porta ${port}`);
});
```

### Descobrir diretorio dinamicamente

```js
import { dirname } from "path";
import { fileURLToPath } from "url";

const __dirname = dirname(fileURLToPath(import.meta.url));

// __dirname + "/public/index.html"
```
