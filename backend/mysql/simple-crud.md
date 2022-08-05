# Mysql Simple CRUD Sheet Cheat

## Sumário
- [`SELECT`](#select)
    - [`DISTINCT`](#DISTINCT)
    - [`LIMIT` e `OFFSET`](#limit-e-offset)
    - [`ORDER BY`](#order-by)
    - [`COUNT`](#count)
- [`WHERE`](#WHERE)
    - [Condições](#Condições)
    - [Operadores Matemáticos](#Operadores-Matemáticos)
    - [Operadores Lógicos](#Operadores-Lógicos)
    - [`IS`](#IS)
    - [`NOT`](#NOT)
    - [`LIKE`](#LIKE)
    - [`BETWEEN`](#BETWEEN)
    - [`IN`](#IN)
- [Datas](#Datas)
- [`INSERT`](#INSERT)
- [`UPDATE`](#UPDATE)
- [`DELETE`](#DELETE)


## `SELECT`

**Template**

```sql
SELECT
    [DISTINCT]
    expressão_de_coluna [, expressão_de_coluna] ...
    [FROM <nome-da-tabela>]
    [WHERE condição]
    [ORDER BY expressão_de_coluna [ASC | DESC]]
    [LIMIT contagem_de_linhas [OFFSET linha_puladas]]
```
> a sintaxe dos colchetes (`[]`) são declarações opcionais 

> sintaxe `expressão [, expressão] ...;` representa que uma expressão é obrigatória, mas pode-se encadear mais expressões separadas por vírgula


---

```sql
SELECT * FROM <nome-do-banco>.<nome-da-tabela>;
SELECT <campo1>, <campo2> FROM <nome-do-banco>.<nome-da-tabela>;
```

**Exemplo**


```sql
SELECT * FROM sakila.actor;
SELECT first_name, last_name FROM sakila.actor;
```

---

### `DISTINCT`

**Template**

```sql
SELECT DISTINCT <campo1> FROM <nome-do-banco>.<nome-da-tabela>;
SELECT DISTINCT <campo1>, <campo2> FROM <nome-do-banco>.<nome-da-tabela>;
```

**Exemplo**

```sql
SELECT DISTINCT first_name FROM sakila.actor;
```

```sql
SELECT DISTINCT first_name, last_name FROM sakila.actor;
```

### `LIMIT` e `OFFSET`

```sql
SELECT * FROM <nome-do-banco>.<nome-da-tabela> LIMIT 10;
SELECT * FROM <nome-do-banco>.<nome-da-tabela> OFFSET 10;
SELECT * FROM <nome-do-banco>.<nome-da-tabela> LIMIT 10 OFFSET 10;
```

**Exemplo**

```sql
SELECT * FROM sakila.actor LIMIT 10;
SELECT * FROM sakila.actor OFFSET 10;
SELECT * FROM sakila.actor LIMIT 10 OFFSET 10;
```

---

### `ORDER BY` 

**Template**

```sql
SELECT * FROM <nome-do-banco>.<nome-da-tabela> ORDER BY <campo1> DESC; -- é ASC por padrão
SELECT * FROM <nome-do-banco>.<nome-da-tabela> ORDER BY <campo1> ASC, <campo2> DESC;
```

**Exemplo**

```sql
SELECT * FROM sakila.actor ORDER BY first_name DESC;
SELECT * FROM sakila.actor ORDER BY first_name DESC, last_name ASC;
SELECT * FROM sakila.payment ORDER BY payment_date ASC;
SELECT * FROM sakila.payment ORDER BY amount DESC;
```

---
### `COUNT`

**Template**

```sql
SELECT COUNT([DISTINCT] coluna [, coluna] ...) FROM <nome-do-banco>.<nome-da-tabela>;
```

**Exemplo**

```sql
SELECT COUNT(*) FROM sakila.film;
SELECT COUNT(DISTINCT first_name) FROM sakila.actor;
SELECT COUNT(DISTINCT first_name, last_name) FROM sakila.actor;
```

---

## `WHERE`

**Template**

```sql
SELECT * FROM <nome-do-banco>.<nome-da-tabela> WHERE condition;
SELECT * FROM <nome-do-banco>.<nome-da-tabela> WHERE true; -- todas as linhas
SELECT * FROM <nome-do-banco>.<nome-da-tabela> WHERE false; -- nenhuma linha
```

---

### Condições

| OPERADOR | DESCRIÇÃO | Exemplo 
|:---:|:----------|:--|
| =   | IGUAL | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> = 1;` |
| >   | MAIOR QUE | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> > 1;` |
| <   | MENOR QUE | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> < 10;`  |
| >=  | MAIOR QUE OU IGUAL | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> >= 10;` |
| <=  | MENOR QUE OU IGUAL  | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> <= 10;` |
| <>  | DIFERENTE DE | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> <> 10;` |
| AND | OPERADOR LÓGICO E | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> < 10 AND <nome-da-tabela>.coluna2 > 7;` |
| OR  | OPERADOR LÓGICO OU | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> < 10 OR <nome-da-tabela>.coluna2 > 7 `|
| NOT | NEGAÇÃO | `SELECT * from <nome-da-tabela> WHERE NOT <nome-da-tabela>.<coluna1>;` |
| IS  | COMPARA COM VALORES BOOLEANOS (TRUE, FALSE, NULL) | `SELECT * from <nome-da-tabela> WHERE <nome-da-tabela>.<coluna1> IS NULL;` |

---

### Operadores Matemáticos

**Exemplos**

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

---

### Operadores Lógicos

**Exemplos**

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

---

### `IS`

**Exemplo**

```sql
SELECT * FROM sakila.staff WHERE picture IS NULL;
```

---

### `NOT`

**Exemplos**

```sql
SELECT * FROM sakila.staff WHERE NOT address_id = 3;
```

```sql
SELECT * FROM sakila.staff WHERE picture IS NOT NULL;
```

---

### `LIKE`
|Sinal| Descrição|
| :--: | :-- |
| `%` | O sinal de percentual, que pode representar zero, um ou múltiplos caracteres
| `_`|  O underscore (às vezes chamado de underline, no Brasil), que representa um único caractere |

**Template**

```sql
SELECT * FROM <nome-do-banco>.<nome-da-tabela> 
    WHERE coluna LIKE '__string%';
```

**Exemplos**

```sql
SELECT * FROM sakila.film
    WHERE title LIKE '%don';
```

```sql
SELECT * FROM sakila.film
    WHERE title LIKE 'plu%';
```

```sql
SELECT * FROM sakila.film
WHERE title LIKE '%plu%';
```

```sql
SELECT * FROM sakila.film
    WHERE title LIKE 'p%r';
```

```sql
SELECT * FROM sakila.film
    WHERE title LIKE '_C%';
```

```sql
SELECT * FROM sakila.film
    WHERE title LIKE '________';
```

---

### `BETWEEN`

**Exemplos**

```sql
SELECT title, length FROM sakila.film
    WHERE length BETWEEN 50 AND 120;
```

```sql
SELECT rental_id, rental_date FROM sakila.rental
    WHERE rental_date
        BETWEEN '2005-05-27' AND '2005-07-17';
```

---

### `IN`

**Exemplos**

```sql
SELECT * FROM sakila.actor
    WHERE first_name IN ('PENELOPE','NICK','ED','JENNIFER');
```

```sql
SELECT * FROM sakila.actor
    WHERE id IN (1, 2, 3, 7, 48, 42);
```

---

## `Datas`

**Exemplos**

```sql
SELECT DATE(payment_date) FROM sakila.payment; -- YYYY-MM-DD
SELECT YEAR(payment_date) FROM sakila.payment; -- Ano
SELECT MONTH(payment_date) FROM sakila.payment; -- Mês
SELECT DAY(payment_date) FROM sakila.payment; -- Dia
SELECT HOUR(payment_date) FROM sakila.payment; -- Hora
SELECT MINUTE(payment_date) FROM sakila.payment; -- Minuto
SELECT SECOND(payment_date) FROM sakila.payment; -- Segundo
```

```sql
SELECT * FROM sakila.payment 
    WHERE YEAR(payment_date) = 2006;
```
```sql
SELECT * FROM sakila.payment
    WHERE payment_date LIKE '2005-07-31%';
```
```sql
SELECT * FROM sakila.payment
    WHERE payment_date 
        BETWEEN '2005-05-26 00:00:00' AND '2005-05-27 23:59:59';
```
```sql
SELECT * FROM sakila.payment 
    WHERE HOUR(payment_date) BETWEEN 10 AND 20;
```

---

## `INSERT`

**Template**

```sql
INSERT [IGNORE]
    [INTO] <nome-da-tabela>
    [(coluna [, coluna] ...)]
    VALUES  (lista_de_valores) [, (lista_de_valores)] ... 

lista_de_valores:
    valor [, valor] ...
```

**Exemplos**

```sql
INSERT INTO sakila.actor (first_name, last_name)
    VALUES ('Bell', 'Hooks');
```
```sql
INSERT INTO sakila.actor (first_name, last_name)
    VALUES ('Bell', 'Hooks'), ('Simone', 'Beauvoir');
```

---

## `UPDATE`

**Template**

```sql
UPDATE [IGNORE] <nome-do-banco>.<nome-da-tabela>
    SET lista_de_atribuições 
    [WHERE condição]
    [ORDER BY ...]
    [LIMIT contagem_de_linhas]

lista_de_atribuições:
    atribuição [, atribuição] ...

atribuição:
    coluna = valor
```
<!--     - UPDATE <nome-do-banco_de_dados>.<nome-da-tabela>
    SET <campo1> = valor1, <campo2> = valor2 — (…) 
    WHERE condição; -->

**Exemplos**

```sql
UPDATE sakila.actor
    SET last_name = 'Bauman'
    WHERE first_name = 'ZERO';
```

```sql
UPDATE sakila.staff
    SET first_name = 'Jean-Paul', last_name = 'Sartre'
    WHERE first_name = 'Mike';
```

⚠️ Caso seja necessário fazer um `UPDATE` sem `WHERE`:

```sql
SET SQL_SAFE_UPDATES = 0;
```

```sql
UPDATE sakila.actor
    SET first_name = 'Nina', last_name = 'Simone';
```

## `DELETE`

**Template**

```sql
DELETE [IGNORE] FROM <nome-da-tabela> 
    [WHERE condição]
    [ORDER BY ...]
    [LIMIT contagem_de_linhas]
```

**Exemplos**

```sql
DELETE FROM sakila.actor
    WHERE first_name = 'NICK';
```


⚠️ Caso seja necessário fazer um `DELETE` sem `WHERE`:

```sql
SET SQL_SAFE_UPDATES = 0;
```

```sql
DELETE FROM sakila.staff;
```
     
