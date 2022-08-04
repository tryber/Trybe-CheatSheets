- Selecionar dados
## SELECT
```sql
SELECT
    [DISTINCT]
    expressão_de_coluna [, expressão_de_coluna] ...
    [FROM tabela]
    [WHERE condição]
    [ORDER BY expressão_de_coluna [ASC | DESC]]
    [LIMIT contagem_de_linhas [OFFSET linha_puladas]]
```
> a sintaxe dos colchetes (`[]`) são declarações opcionais 

> sintaxe `expressão [, expressão] ...;` representa que uma expressão é obrigatória, mas pode-se encadear mais expressões separadas por vírgula


```sql
SELECT * FROM banco.tabela;
SELECT campo1, campo2 FROM banco.tabela;
```

```sql
SELECT * FROM sakila.actor;
SELECT first_name, last_name FROM sakila.actor;
```

### DISTINCT
```sql
SELECT DISTINCT campo1 FROM banco.tabela;
SELECT DISTINCT campo1, campo2 FROM banco.tabela;
```
```sql
SELECT DISTINCT first_name FROM sakila.actor;
```
```sql
SELECT DISTINCT first_name, last_name FROM sakila.actor;
```
### LIMIT e OFFSET

```sql
SELECT * FROM banco.tabela LIMIT 10;
SELECT * FROM banco.tabela OFFSET 10;
SELECT * FROM banco.tabela LIMIT 10 OFFSET 10;
```

```sql
SELECT * FROM sakila.actor LIMIT 10;
SELECT * FROM sakila.actor OFFSET 10;
SELECT * FROM sakila.actor LIMIT 10 OFFSET 10;
```

### ORDER BY 

```sql
SELECT * FROM banco ORDER BY field1 DESC; -- é ASC por padrão
SELECT * FROM banco ORDER BY campo1 ASC, campo2 DESC;
```

## WHERE
```sql
SELECT * FROM banco WHERE condition;
SELECT * FROM banco WHERE true;
```

### Condições
- Operadores Booleanos e Lógicos

| OPERADOR | DESCRIÇÃO | Exemplo 
|:---:|:----------|:--|
| =   | IGUAL | `SELECT * from tabela WHERE tabela.coluna1 = 1;` |
| >   | MAIOR QUE | `SELECT * from tabela WHERE tabela.coluna1 > 1;` |
| <   | MENOR QUE | `SELECT * from tabela WHERE tabela.coluna1 < 10;`  |
| >=  | MAIOR QUE OU IGUAL | `SELECT * from tabela WHERE tabela.coluna1 >= 10;` |
| <=  | MENOR QUE OU IGUAL  | `SELECT * from tabela WHERE tabela.coluna1 <= 10;` |
| <>  | DIFERENTE DE | `SELECT * from tabela WHERE tabela.coluna1 <> 10;` |
| AND | OPERADOR LÓGICO E | `SELECT * from tabela WHERE tabela.coluna1 < 10 AND tabela.coluna2 > 7;` |
| OR  | OPERADOR LÓGICO OU | `SELECT * from tabela WHERE tabela.coluna1 < 10 OR tabela.coluna2 > 7 `|
| NOT | NEGAÇÃO | `SELECT * from tabela WHERE NOT tabela.coluna1;` |
| IS  | COMPARA COM VALORES BOOLEANOS (TRUE, FALSE, NULL) | `SELECT * from tabela WHERE tabela.coluna1 IS NULL;` |

### Operadores Matemáticos
```sql
SELECT * FROM sakila.payment WHERE id = 1;
```

```sql
SELECT * FROM sakila.payment WHERE amount < 10;
```

```sql
SELECT * FROM sakila.payment WHERE amount >= 0.99;
```

```sql
SELECT * FROM sakila.language WHERE name <> 'English';
```

### Operadores Lógicos
```sql
SELECT * FROM sakila.payment
WHERE amount = 0.99 AND staff_id = 2;
```

```sql
SELECT * FROM sakila.payment
WHERE amount = 0.99 OR amount = 2.99;
```

```sql
SELECT * FROM sakila.payment
WHERE amount = 0.99 OR amount = 2.99 AND staff_id = 2;
```

### `IS`
```sql
SELECT * FROM sakila.staff WHERE picture IS NULL;
```


### `NOT`
```sql
SELECT * FROM sakila.staff WHERE NOT address_id = 3;
```
```sql
SELECT * FROM sakila.staff WHERE picture IS NOT NULL;
```

## `INSERT`
- Inserir dados
    - INSERT INTO banco_de_dados.tabela (campo1, campo2)
    VALUES (valor1,valor2), (valor3,valor4) —(…)
    ;
## `UPDATE`
```sql
UPDATE [IGNORE] banco.tabela
    SET lista_de_atribuições 
    [WHERE condição]
    [ORDER BY ...]
    [LIMIT contagem_de_linhas]

lista_de_atribuições:
    atribuição [, atribuição] ...

atribuição:
    coluna = valor
```
<!--     - UPDATE banco_de_dados.tabela
    SET campo1 = valor1, campo2 = valor2 — (…) 
    WHERE condição; -->

```sql
UPDATE sakila.staff
    SET first_name = 'Rannveig'
    WHERE first_name = 'Ravein';
```
 Caso seja necessário fazer um `UPDATE` sem `WHERE`
```sql
SET SQL_SAFE_UPDATES = 0;
```

## `DELETE`

- Deletar dados
    - DELETE FROM banco_de_dados.tabela
    WHERE coluna = 'valor';
     
- Funções
