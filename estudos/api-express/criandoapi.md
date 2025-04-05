# CRIANDO API REST

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
