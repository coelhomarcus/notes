# Postgress

## Aqui vai ter revisão tanto de SQL quando do Postgress

### Criando Tabela - SQL

[Tipos de Dados - SQL Postgres](https://www.postgresql.org/docs/current/datatype.html)

```sql
CREATE TABLE Amigos (
    id INT PRIMARY KEY,
    nome VARCHAR(50),
    idade INT
);
```

### Inserindo Dados - SQL

```sql
-- Sempre usar aspas simples em string!! Se não da erro
INSERT INTO amigos VALUES (0, 'Marcus', 19);
```
