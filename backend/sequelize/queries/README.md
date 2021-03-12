# Sequelize Queries Cheat Sheet

# Sumário

- [Sequelize Queries Cheat Sheet](#sequelize-queries-cheat-sheet)
- [Sumário](#sumário)
- [Operadores](#operadores)
- [Métodos](#métodos)
  - [create](#create)
  - [update](#update)
  - [destroy](#destroy)
  - [findAll](#findall)
    - [SELECT simples](#select-simples)
    - [SELECT específico](#select-específico)
      - [Sem alias](#sem-alias)
      - [Com alias](#com-alias)
    - [SELECT Agregações](#select-agregações)
    - [SELECT com WHERE](#select-com-where)
    - [GROUP BY](#group-by)
    - [LIMIT / OFFSET](#limit--offset)
  - [findByPK](#findbypk)
  - [findOne](#findone)
  - [findOrCreate](#findorcreate)
  - [findAndCountAll](#findandcountall)
  - [count](#count)
  - [max](#max)
  - [min](#min)
  - [sum](#sum)

---

# Operadores

```javascript
const { Op } = require("sequelize");
await Post.findAll({
  where: {
    [Op.and]: [{ a: 5 }, { b: 6 }],            // (a = 5) AND (b = 6)
    [Op.or]: [{ a: 5 }, { b: 6 }],             // (a = 5) OR (b = 6)
    someAttribute: {
      // Basics
      [Op.eq]: 3,                              // = 3
      [Op.ne]: 20,                             // != 20
      [Op.is]: null,                           // IS NULL
      [Op.not]: true,                          // IS NOT TRUE
      [Op.or]: [5, 6],                         // (someAttribute = 5) OR (someAttribute = 6)

      // Using dialect specific column identifiers (PG in the following example):
      [Op.col]: 'user.organization_id',        // = "user"."organization_id"

      // Number comparisons
      [Op.gt]: 6,                              // > 6
      [Op.gte]: 6,                             // >= 6
      [Op.lt]: 10,                             // < 10
      [Op.lte]: 10,                            // <= 10
      [Op.between]: [6, 10],                   // BETWEEN 6 AND 10
      [Op.notBetween]: [11, 15],               // NOT BETWEEN 11 AND 15

      // Other operators

      [Op.all]: sequelize.literal('SELECT 1'), // > ALL (SELECT 1)

      [Op.in]: [1, 2],                         // IN [1, 2]
      [Op.notIn]: [1, 2],                      // NOT IN [1, 2]

      [Op.like]: '%hat',                       // LIKE '%hat'
      [Op.notLike]: '%hat',                    // NOT LIKE '%hat'
      [Op.startsWith]: 'hat',                  // LIKE 'hat%'
      [Op.endsWith]: 'hat',                    // LIKE '%hat'
      [Op.substring]: 'hat',                   // LIKE '%hat%'
      [Op.iLike]: '%hat',                      // ILIKE '%hat' (case insensitive) (PG only)
      [Op.notILike]: '%hat',                   // NOT ILIKE '%hat'  (PG only)
      [Op.regexp]: '^[h|a|t]',                 // REGEXP/~ '^[h|a|t]' (MySQL/PG only)
      [Op.notRegexp]: '^[h|a|t]',              // NOT REGEXP/!~ '^[h|a|t]' (MySQL/PG only)
      [Op.iRegexp]: '^[h|a|t]',                // ~* '^[h|a|t]' (PG only)
      [Op.notIRegexp]: '^[h|a|t]',             // !~* '^[h|a|t]' (PG only)

      [Op.any]: [2, 3],                        // ANY ARRAY[2, 3]::INTEGER (PG only)
    }
  }
});
```

# Métodos

## create

**Template**

```javascript
await Model.create({
  <field1>: <value1>,
  ...,
  <fieldN>: <valueN>,
});
```

**Exemplo**

```javascript
await User.create({
  fullName: "Jane Doe",
  email: "jane.doe@gmail.com",
});
```

**Query MySQL Equivalente**

```sql
INSERT INTO Users('fullName', 'email')
VALUES ('Jane Doe', 'jane.doe@gmail.com');
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#simple-insert-queries)

[Voltar para Sumário](#sumário)

---

## update

**Template**

```javascript
await Model.update(
  {
    <campo>: <valor>,
    ...,
    <campoN>: <valorN>
  },
  {
    where: { <expressão de filtragem> },
  },
);
```

**Exemplo**

```javascript
await User.update(
  { lastName: "Doe" },
  {
    where: { lastName: null },
  },
);
```

**Query MySQL Equivalente**

```sql
UPDATE user
SET lastName = 'Doe'
WHERE lastName = 'null';
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#simple-update-queries)

[Voltar para Sumário](#sumário)

---

## destroy

**Template**

```javascript
await Model.destroy({
  where: {
    <campo>: <valor>,
  },
});
```

**Exemplo**

```javascript
await User.destroy({
  where: {
    firstName: "Jane",
  },
});
```

**Query MySQL Equivalente**

```sql
DELETE FROM User
WHERE firstname = 'Jane';
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#simple-delete-queries)

[Voltar para Sumário](#sumário)

---

## findAll

### SELECT simples

**Template**

```javascript
await Model.findAll();
```

**Exemplo**

```javascript
await User.findAll();
```

**Query MySQL Equivalente**

```sql
SELECT * FROM User;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#simple-select-queries)

[Voltar para Sumário](#sumário)

---

### SELECT específico

#### Sem alias

**Template**

```javascript
await Model.findAll({
  attributes: [ <atributo01>, <atributo02> ],
});
```

**Exemplo**

```javascript
await Products.findAll({
  attributes: [ 'price', 'stock' ],
});
```

**Query MySQL Equivalente**

```sql
SELECT price, stock FROM Products;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#specifying-attributes-for-select-queries)

[Voltar para Sumário](#sumário)

---

#### Com alias

**Template**

```javascript
await Model.findAll({
  attributes: [ <atributo01>, [ <atributo02>, <alias> ], ..., <atributo03>]
});
```

**Exemplo**

```javascript
await Products.findAll({
  attributes: ['price', ['product_name', 'name'], 'stock']
});
```

**Query MySQL Equivalente**

```sql
SELECT price, product_name AS name, stock FROM Products;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#specifying-attributes-for-select-queries)

[Voltar para Sumário](#sumário)

---

### SELECT Agregações

**Template**

```javascript
await Model.findAll({
  attributes: [
    <attribute01>,
    [ sequelize.fn(<metodo>, sequelize.col(<coluna>)), <alias> ],
    ...,
    <attributeN>,
  ],
});
```

**Exemplo**

```javascript
await Products.findAll({
  attributes: [
    'foo',
    [sequelize.fn('COUNT', sequelize.col('hats')), 'n_hats'],
    'bar'
  ]
});
```

**Query MySQL Equivalente**

```sql
SELECT foo, COUNT(hats) AS n_hats, bar FROM Products;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#specifying-attributes-for-select-queries)

[Voltar para Sumário](#sumário)

---

### SELECT com WHERE

**Template**

```javascript
await Model.findAll({
  where: {
    <campo1>: <valor>,
    ...,
    <campoN>: <valor>,
  },
});
```

**Exemplo**

```javascript
await Post.findAll({
  where: {
    authorId: 2,
    status: 'active'
  }
});
```

**Query MySQL Equivalente**

```sql
SELECT * FROM post WHERE authorId = 2 AND status = 'active';
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#applying-where-clauses)

[Voltar para Sumário](#sumário)

---

### GROUP BY

**Template**

```javascript
await Model.findAll({ group: <campo> });
```

**Exemplo**

```javascript
await Project.findAll({ group: 'name' });
```

**Query MySQL Equivalente**

```sql
GROUP BY name;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#grouping)

[Voltar para Sumário](#sumário)

---

### LIMIT / OFFSET

**Template**

```javascript
// limit
await Model.findAll({ limit: <numero> });
```

```javascript
// offset
await Model.findAll({ offset: <numero> });
```

```javascript
// limit & offset
await Model.findAll({ offset: <numero1>, limit: <numero2> });
```

**Exemplo**

```javascript
// limit
await Project.findAll({ limit: 10 });
```

```javascript
// offset
await Project.findAll({ offset: 8 });
```

```javascript
// limit & offset
await Project.findAll({ offset: 5, limit: 5 });
```

**Query MySQL Equivalente**

```sql
-- limit
SELECT * FROM Project
LIMIT 10;
```

```sql
-- offset
SELECT * FROM Project
OFFSET 8;
```

```sql
-- limit & offset
SELECT * FROM Project
OFFSET 5
LIMIT 10;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#limits-and-pagination)

[Voltar para Sumário](#sumário)

---


## findByPK

Busca por um resultado, usando sua chave primária.

**Template**

```javascript
const results = await Model.findByPk(valor);
if (project === null) {
  console.log('Not found!');
} else {
  console.log(results instanceof Model); // true
  //  a pk é <valor>
}
```

**Exemplo**

```javascript
const project = await Project.findByPk(123);
if (project === null) {
  console.log('Not found!');
} else {
  console.log(project instanceof Project); // true
  //  a pk é 123
}
```

[Documentação](https://sequelize.org/master/manual/model-querying-finders.html#-code-findbypk--code-)

[Voltar para Sumário](#sumário)

---

## findOne

Busca o primeiro resultado filtrado pela busca.

**Template**

```javascript
const results = await Model.findOne({ where: { campo: valor } });
if (results === null) {
  console.log('Not found!');
} else {
  console.log(results instanceof Model); // true
  console.log(results.campo); // valor
}
```

**Exemplo**

```javascript
const project = await Project.findOne({ where: { title: 'Título' } });
if (project === null) {
  console.log('Not found!');
} else {
  console.log(project instanceof Project); // true
  console.log(project.title); // 'Título'
}
```

[Documentação](https://sequelize.org/master/manual/model-querying-finders.html#-code-findone--code-)

[Voltar para Sumário](#sumário)

---

## findOrCreate

Criará uma entrada na tabela, a menos que encontre um resultado que preencha as opções de consulta.

> Obs.: Em ambos os casos, ele retornará uma instância (a instância encontrada ou a instância criada) e um booleano indicando se essa instância foi criada ou já existia.

**Template**

```javascript
const [file, created] = await Model.findOrCreate({
  where: { filename: nome },
  defaults: {
    campo: valor
  }
});
console.log(file.filename);
console.log(file.campo);
console.log(created);
if (created) {
  console.log(file.campo);
}
```

**Exemplo**

```javascript
const [user, created] = await User.findOrCreate({
  where: { username: 'Eric' },
  defaults: {
    job: 'Technical Lead JavaScript'
  }
});
console.log(user.username); // 'Eric'
console.log(user.job); // pode ou não ser 'Technical Lead JavaScript'
console.log(created); // Valor booleano que indica se a instancia foi criada ou nao
if (created) {
  console.log(user.job); // 'Technical Lead JavaScript'
}
```

[Documentação](https://sequelize.org/master/manual/model-querying-finders.html#-code-findorcreate--code-)

[Voltar para Sumário](#sumário)

---

## findAndCountAll

Método conveniente que combina findAll e count.

**Template**

```javascript
const { count, rows } = await Model.findAndCountAll({
  where: {
    campo: {
      [Op.<operação>]: 'foo%'
    }
  }
});
```

**Exemplo**

```javascript
const { Op } = require("sequelize");

const { count, rows } = await Project.findAndCountAll({
  where: {
    title: {
      [Op.like]: 'foo%'
    }
  },
  offset: 10,
  limit: 2
});

console.log(count);
console.log(rows);
```

[Documentação](https://sequelize.org/master/manual/model-querying-finders.html#-code-findandcountall--code-)

[Sessão Operadores](#operadores)

[Voltar para Sumário](#sumário)

---

## count

**Template**

```javascript
// simples
await Model.count();
```

```javascript
// com filtro
const { Op } = require("sequelize");

await Model.count({
  where: {
    <campo>: {
      [Op.<operação>]: <valor>,
    },
  },
});
```

**Exemplo**

```javascript
// com filtro
const { Op } = require("sequelize");

await Project.count({
  where: {
    id: {
      [Op.gt]: 25
    }
  }
});
```

**Query MySQL Equivalente**

```sql
SELECT COUNT('id') from project
WHERE 'id' > 25;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#-code-count--code-)

[Sessão Operadores](#operadores)

[Voltar para Sumário](#sumário)

---

## max

**Template**

```javascript
await Model.max(<coluna>);
```

**Exemplo**

```javascript
await User.max('age');
```

```javascript
// com filtragem
const { Op } = require("sequelize");

await User.max('age', {
  where: {
     age: { [Op.lt]: 20 },
  },
});
```

**Query MySQL Equivalente**

```sql
SELECT MAX(age) FROM User;
```

```sql
--- com filtagem
SELECT MAX(age)
FROM User
WHERE age < 20;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#-code-max--code----code-min--code--and--code-sum--code-)

[Sessão Operadores](#operadores)

[Voltar para Sumário](#sumário)

---

## min

**Template**

```javascript
await Model.min(<coluna>);
```

**Exemplo**

```javascript
await User.min('age');
```

```javascript
// com filtragem
const { Op } = require("sequelize");

await User.min('age', {
  where: {
     age: { [Op.gt]: 5 },
  },
});
```

**Query MySQL Equivalente**

```sql
SELECT MIN(age) FROM User;
```

```sql
--- com filtagem
SELECT MIN(age)
FROM User
WHERE age > 5;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#-code-max--code----code-min--code--and--code-sum--code-)

[Sessão Operadores](#operadores)

[Voltar para Sumário](#sumário)

---

## sum

**Template**

```javascript
await Model.sum(<coluna>);
```

**Exemplo**

```javascript
await User.sum('age');
```

```javascript
// com filtragem
const { Op } = require("sequelize");

await User.sum('age', {
  where: {
     age: { [Op.gt]: 5 },
  },
});
```

**Query MySQL Equivalente**

```sql
SELECT SUM(age) FROM User;
```

```sql
--- com filtagem
SELECT SUM(age)
FROM User
WHERE age > 5;
```

[Documentação](https://sequelize.org/master/manual/model-querying-basics.html#-code-max--code----code-min--code--and--code-sum--code-)

[Sessão Operadores](#operadores)

[Voltar para Sumário](#sumário)

---
