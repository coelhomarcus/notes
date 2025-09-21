# PostgreSQL

## Criando Tabela - SQL

[Tipos de Dados - SQL Postgres](https://www.postgresql.org/docs/current/datatype.html)

```sql
CREATE TABLE Amigos (
    id INT PRIMARY KEY,
    nome VARCHAR(50),
    idade INT
);
```

## Inserindo Dados - SQL

> Sempre usar aspas simples em string!! Se não da erro

```sql
INSERT INTO amigos VALUES (0, 'Marcus', 19);
```

> No futuro adicionarei uma pasta apenas para `Postgress`, com mais informações.
