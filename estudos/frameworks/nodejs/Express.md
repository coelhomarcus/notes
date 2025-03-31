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

### Utilizando body-parser (middleware)

```js
import express from "express";
import bodyParser from "body-parser";
import { dirname } from "path";
import { fileURLToPath } from "url";
const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

//sem isso o body poderia vir undefined
//mas ainda tenho que estudar as funcionalidades direito
app.use(bodyParser.urlencoded({ extended: true }));

//ao entrar no localhost:3000 ele carregara o index.html
app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

//la no HTML tem o form com action="/submit" method="POST"
app.post("/submit", (req, res) => {
  console.log(req.body);
});

app.listen(port, () => {
  console.log(`Servidor rodando na porta ${port}`);
});
```
