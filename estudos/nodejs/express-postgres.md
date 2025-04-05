# Express com Postgres

```js
import express from "express";
import pg from "pg";

let amigos;

const app = express();
app.use(express.urlencoded({ extended: true }));

const db = new pg.Client({
  user: "coelhomarcus",
  host: "localhost",
  database: "coelhomarcus",
  password: "123",
  port: 5432,
});

db.connect();

db.query("SELECT * FROM amigos", (err, res) => {
  if (err) {
    console.log(err.stack);
  } else {
    amigos = res.rows;
  }
});

app.get("/", (req, res) => {
  res.json(amigos);
});

app.post("/add", (req, res) => {
  db.query("INSERT INTO amigos VALUES ($1, $2, $3)", [
    req.body.id,
    req.body.nome,
    req.body.idade,
  ]);
  res.json("Deu certo");
});

app.listen(3000, () => {
  console.log("Rodando na porta 3000");
});
```
