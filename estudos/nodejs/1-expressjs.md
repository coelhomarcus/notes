### Express

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

### Criando o proprio Middleware

```js
import express from "express";

const app = express();

//Criando meu proprio Middleware
function logger(req, res, next) {
  //Apenas testes
  console.log("O URL da requisição é: " + req.url);
  console.log("O METHOD da requisição é: " + req.method);
  //passa para o prodixo middleware ou continua o codigo
  //se esquecer do next(); a requisição vai carregar infinito e nao ira concluir
  next();
}

//Utilizando o middleware
app.use(logger);

app.get("/", (req, res) => {
  res.send("Olá");
});

app.listen(3000, () => {
  console.log("Servidor rodando na porta 3000");
});
```

### Criando outra API

```js
import express from "express";

let jokes = [
  {
    id: 0,
    joke: "Por que o programador foi ao médico? Porque ele tinha um bug no sistema.",
    autor: "Piada Man",
  },
  {
    id: 1,
    joke: "O que o 0 disse pro 8? Bonito cinto!",
    autor: "Piada Man",
  },
  {
    id: 2,
    joke: "Como o JavaScript termina uma briga? Com um try...catch!",
    autor: "Piada Man",
  },
  {
    id: 3,
    joke: "Por que o computador foi preso? Porque ele executou um código malicioso.",
    autor: "Piada Man",
  },
  {
    id: 4,
    joke: "O que um array disse pro outro? Me inclui nessa!",
    autor: "Piada Man",
  },
];

const apiKey = "senha";
const min = 0;
const max = jokes.length - 1;

const app = express();

//usado para suportar parametros no x-www-encoded
app.use(express.urlencoded({ extended: true }));

//Pegar todas as piadas
app.get("/", (req, res) => {
  res.send(jokes);
});

//Pegar uma piada aleatorioa
app.get("/random", (req, res) => {
  const randomIndex = Math.floor(Math.random() * (max - min + 1)) + min;
  res.send(JSON.stringify(jokes[randomIndex]));
});

//Pegar uma piada especifica pelo ID
app.get("/joke/:id", (req, res) => {
  res.send(JSON.stringify(jokes[req.params.id]));
});

//Adicionando uma piada passando a piada e o autor
app.post("/add", (req, res) => {
  const newId = jokes.length;
  const newJoke = req.body.joke;
  const newAutor = req.body.autor;

  const newJokes = {
    id: newId,
    joke: newJoke,
    autor: newAutor,
  };

  jokes.push(newJokes);

  res.json({
    ...newJokes,
    stats: "Nova requisição enviada com sucesso!",
  });
});

//Modificando uma piada existente
app.put("/joke/:id", (req, res) => {
  jokes[req.params.id].autor = req.body.autor;
  jokes[req.params.id].joke = req.body.joke;

  res.json(jokes[req.params.id]);
});

//Deletar por ID
app.delete("/joke/:id", (req, res) => {
  const searchIndex = jokes.findIndex((joke) => joke.id == req.params.id);

  if (searchIndex < 0) {
    res.sendStatus(404);
  } else {
    jokes.splice(searchIndex, 1);
    res.sendStatus(200);
  }
});

//Deletar todos com AUTORIZAÇÃO
app.delete("/all", (req, res) => {
  if (apiKey === req.query.key) {
    jokes = [];
    res.json(jokes);
  } else {
    res.json(`Chave invalida`);
  }
});

app.listen(3000, () => {
  console.log("Servidor rodando na porta 3000");
});
```

---

## 📚 Navegação

| **← Anterior** | **Atual** | **Próximo →** |
|---|---|---|
| [Node.js Básico](./0-nodejs.md) | **Express.js** | [PostgreSQL](./2-postgresql.md) |
