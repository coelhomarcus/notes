# Express

## Instalação

```bash
npm init -y

npm install express
```

Mudando o arquivo `main` em `package.json` para `index.js`. Também mudando o tipo para usar ES Modules:

```json
{
  "main": "index.js",
  "type": "module"
}
```

## `Nodemon`

Vamos usar para executar e não precisar ficar fechando e rodando de novo com alterações `nodemon index.js`

```bash
npm install -g nodemon
```

> Hoje já existe `--watch` no node, então não é necessário, mas `Nodemon` ainda é muito usado.

## Criando Aplicação

> Exemplo básico de um servidor Express com rotas para todos os métodos HTTP principais (GET, POST, PUT, DELETE).

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

## Descobrir diretório dinamicamente

> Como obter o caminho do diretório atual em ES Modules. Necessário para servir arquivos estáticos ou referenciar caminhos relativos.

```js
import { dirname } from "path";
import { fileURLToPath } from "url";

const __dirname = dirname(fileURLToPath(import.meta.url));

// __dirname + "/public/index.html"
```

## Utilizando `body-parser` (middleware)

> Middleware para processar dados de formulários HTML codificados em formato `application/x-www-form-urlencoded`. Este é o formato padrão que navegadores usam para enviar dados de formulários (ex: `nome=João&idade=25`). Sem esse middleware, `req.body` ficaria `undefined` em requisições POST.

```js
import express from "express";
import bodyParser from "body-parser";
import { dirname } from "path";
import { fileURLToPath } from "url";
const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

app.use(bodyParser.urlencoded({ extended: true }));

// Rota principal - serve o arquivo HTML com formulário
app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

// Rota para processar dados do formulário HTML (action="/submit" method="POST")
app.post("/submit", (req, res) => {
  console.log(req.body); // Aqui conseguimos acessar os dados do formulário
});

app.listen(port, () => {
  console.log(`Servidor rodando na porta ${port}`);
});
```

## Criando nosso `Middleware`

```js
import express from "express";

const app = express();

// Criando Middleware
function logger(req, res, next) {
  //Apenas testes
  console.log("O URL da requisição é: " + req.url);
  console.log("O METHOD da requisição é: " + req.method);

  // IMPORTANTE: next() passa o controle para o próximo middleware
  // Sem chamar next(), a requisição fica pendente e o servidor "trava"
  next();
}

// Utilizando o middleware
app.use(logger);

app.get("/", (req, res) => {
  res.send("Olá");
});

app.listen(3000, () => {
  console.log("Servidor rodando na porta 3000");
});
```

## Refatorando Rotas

`Antes`
```js
import express from "express";

const app = express();

app.get("/api/products", (req, res) => {
    res.json({ data: "products" });
});

app.get("/api/services", (req, res) => {
    res.json({ data: "service" });
});

app.listen(8000, () => console.log("listening 8000"));
```

`Depois`
```js
import express from "express";

const app = express();

const apiRouter = express.Router();

const productsController = (req, res) => {
    res.json({ data: "products" });
};

const servicesController = (req, res) => {
    res.json({ data: "service" });
};

apiRouter.get("/products", productsController);
apiRouter.get("/services", servicesController);

app.use("/api", apiRouter);

app.listen(8000, () => console.log("listening 8000"));
```

> So que desse jeito no mesmo arquivo ainda nao é certo, o certo é modularizar o codigo dessa forma a seguir:

## Modularizando
> Entao criamos as pastas `controllers` e `routes`

```js
//controllers/productsController.js
export const productsController = (req, res) => {
    res.json({ data: "products" });
};
```

```js
//controllers/servicesController.js
export const servicesController = (req, res) => {
    res.json({ data: "service" });
};
```

```js
//routes/apiRoutes.js
import express from "express";
import { productsController } from "../controllers/productsController.js";
import { servicesController } from "../controllers/servicesController.js";

export const apiRouter = express.Router();
apiRouter.get("/products", productsController);
apiRouter.get("/services", servicesController);
```

```js
//server.js
import express from "express";
import { apiRouter } from "./routes/apiRouter.js";

const app = express();

app.use("/api", apiRouter);

// 404 para qualquer rota que não exista.
app.use((req, res) => {
    res.status(404).json({ message: "Endpoint not found." });
});

app.listen(8000, () => console.log("listening 8000"));
```


## Criando outra API

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

// Usado para suportar parametros no x-www-encoded
app.use(express.urlencoded({ extended: true }));

// Pegar todas as piadas
app.get("/", (req, res) => {
  res.send(jokes);
});

// Pegar uma piada aleatorioa
app.get("/random", (req, res) => {
  const randomIndex = Math.floor(Math.random() * (max - min + 1)) + min;
  res.send(JSON.stringify(jokes[randomIndex]));
});

// Pegar uma piada especifica pelo ID
app.get("/joke/:id", (req, res) => {
  res.send(JSON.stringify(jokes[req.params.id]));
});

// Adicionando uma piada passando a piada e o autor
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

// Modificando uma piada existente
app.put("/joke/:id", (req, res) => {
  jokes[req.params.id].autor = req.body.autor;
  jokes[req.params.id].joke = req.body.joke;

  res.json(jokes[req.params.id]);
});

// Deletar por ID
app.delete("/joke/:id", (req, res) => {
  const searchIndex = jokes.findIndex((joke) => joke.id == req.params.id);

  if (searchIndex < 0) {
    res.sendStatus(404);
  } else {
    jokes.splice(searchIndex, 1);
    res.sendStatus(200);
  }
});

// Deletar todos com AUTORIZAÇÃO
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
