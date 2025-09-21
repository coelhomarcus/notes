# Express com PostgreSQL

> Para sistemas de autenticação mais avançados, é possível implementar login social (`Google`, `GitHub`, etc) e sessões persistentes com cookies. Biblioteca recomendada: `Passport.js`.

> ⚠️ **`IMPORTANTE`:** O código abaixo usa queries SQL diretas com placeholders (`$1`, `$2`), que são **seguros contra SQL Injection**. Nunca concatene strings diretamente nas queries SQL (ex: `"SELECT * FROM users WHERE id = " + userId`) pois isso permite ataques maliciosos. Sempre use parâmetros ou ORMs como Prisma/Sequelize para maior segurança.

```js
import express from "express";
import pg from "pg";

let amigos;

const app = express();
app.use(express.urlencoded({ extended: true }));

// Passando os dados do database e fazendo a conexão.
const db = new pg.Client({
  user: "coelhomarcus",
  host: "localhost",
  database: "coelhomarcus",
  password: "123",
  port: 5432,
});

db.connect();

// Testando Query
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

## Autenticação com BCRYPT e HASH

```js
import express from "express";
import pg from "pg";
import bcrypt from "bcrypt";

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

const saltRounds = 10;

// Requisição POST simples de registo com EMAIL E SENHA
// nessa requisição fazemos um insert no banco de dados
// onde pegamos o email e password do body da requisição post
// usamos o bcrypt para transformar a senha em hash
// no bcrypt os parametros são: senha, saltRounds, callback (com error e o hash que é a senha convertida)
// então fazemos o insert no BD com o hash ao inves da senha
// utilizamos async await para conseguir pegar a resposta das querys e poder usar o try catch para tratar alguns erros
app.post("/register", (req, res) => {
  const email = req.body.email;
  const password = req.body.password;

  bcrypt.hash(password, saltRounds, async (err, hash) => {
    if (err) {
      console.log(err);
      return res.status(500).json({ error: "Erro ao gerar hash da senha." });
    }

    try {
      await db.query("INSERT INTO users VALUES ($1, $2)", [email, hash]);
      const response = await db.query("SELECT * FROM users WHERE email = $1", [
        email,
      ]);
      res.json(response.rows);
    } catch (error) {
      if (error.code === "23505") {
        // 23505 = unique_violation em PostgreSQL
        res.status(400).json({ error: "E-mail já cadastrado." });
      } else {
        console.error(error);
        res.status(500).json({ error: "Erro interno ao cadastrar usuário." });
      }
    }
  });
});

app.listen(3000, () => {
  console.log("Servidor rodando na porta 3000");
});
```

## Comparação de SENHA com HASH - BCRYPT

```js
bcrypt(senhaINPUT, senhaHASH, (err, result) => {
  if (result) {
    //resto é contigo
  }
});
```
