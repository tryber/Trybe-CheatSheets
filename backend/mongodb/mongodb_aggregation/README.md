# MongoDB Aggregation Cheat Sheet

# Sumário

- [MongoDB Aggregation Cheat Sheet](#mongodb-aggregation-cheat-sheet)
- [Sumário](#sumário)
- [Operadores](#operadores)
  - [Operadores Aggregation](#operadores-aggregation)
    - [$match](#match)
    - [$limit](#limit)
    - [$group](#group)
    - [$project](#project)
    - [$unwind](#unwind)
    - [$lookup](#lookup)
    - [$lookup (let/pipeline)](#lookup-letpipeline)
    - [$addFields](#addfields)
  - [Operadores Aritméticos](#operadores-aritméticos)
    - [$add](#add)
    - [$subtract](#subtract)
    - [$ceil](#ceil)
    - [$floor](#floor)
    - [$abs](#abs)
    - [$multiply](#multiply)
    - [$divide](#divide)
  - [Operadores Comparativos](#operadores-comparativos)
    - [$lt](#lt)
    - [$lte](#lte)
    - [$gt](#gt)
    - [$gte](#gte)
    - [$eq](#eq)
    - [$ne](#ne)
    - [$in](#in)
    - [$nin](#nin)
  - [Operadores Lógicos](#operadores-lógicos)
    - [$not](#not)
    - [$or](#or)
    - [$nor](#nor)
    - [$and](#and)

---

# Operadores

## Operadores Aggregation

### $match

**Template**

```
db.collection.aggregate([
  { $match: { <query> } },
]);
```

**Exemplo**

```javascript
db.workers.aggregate([
  { $match: { workerName: "Tiago" } },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/match/)

[Voltar para Sumário](#sumário)

---

### $limit

**Template**

```
db.collection.aggregate([
  { $limit: <inteiro positivo> },
]);
```

**Exemplo**

```javascript
db.products.aggregate([
	{ $match: { laptop: 'Dell' } },
    { $limit: 5 },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/limit/)

[Voltar para Sumário](#sumário)

---

### $group

**Template**

```
db.collection.aggregate([
	{
		$group: {
		  _id: <expressão>,
			<campo1>: { <acumulador1> : <expressão1> },
			...
			<campoN>: { <acumuladorN> : <expressãoN> },
		},
	},
]);
```

**Exemplo**

```javascript
db.products.aggregate([
  {
    $group : {
      _id : "$laptopId",
      count: { $sum: 1 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/group/)

[Voltar para Sumário](#sumário)

---

### $project

**Template**

```
db.collection.aggregate([
  {
    project: {
      <especificação(ões)>
    },
  },
]);
```

**Exemplo**

```javascript
db.products.aggregate([
  {
    $project: {
      _id: 0, // ou false
      productName: "$laptop",
      quantity: 1, // ou true
      profit: {
        $subtract: ["$sale_price", "$cost_price"]
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/project)

[Voltar para Sumário](#sumário)

---

### $unwind

**Template**

```
db.collection.aggregate([
  { $unwind: <caminho do campo array> },
]);
```

**Exemplo**

```javascript
db.streamings.aggregate([
  { $unwind: "$netflix_plans" },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/unwind/)

[Voltar para Sumário](#sumário)

---

### $lookup

**Template**

```
db.collection.aggregate([
	{
    $lookup: {
      from: <coleção para unir>,
      localField: <campo dos documentos de entrada>,
      foreignField: <campo dos documentos provenientes da coleção conectada,
      as: <campo do array de saída>
    },
  },
]);
```

**Exemplo**

```javascript
db.orders.aggregate([
	{
    $lookup: {
      from: "inventory",
      localField: "item",
      foreignField: "sku",
      as: "inventory_docs"
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/)

[Voltar para Sumário](#sumário)

---

### $lookup (let/pipeline)

**Template**

```
db.collection.aggregate([
  {
   $lookup:
     {
       from: <coleção para unir>,
       let: { <var_1>: <expressão>, …, <var_n>: <expressão> },
       pipeline: [ <pipeline a ser executada na coleção unida> ],
       as: <campo do array de saída>
     }
}
]);
```

**Exemplo**

```javascript
db.orders.aggregate([
   {
      $lookup:
         {
           from: "warehouses",
           let: { order_item: "$item", order_qty: "$ordered" },
           pipeline: [
              { $match:
                 { $expr:
                    { $and:
                       [
                         { $eq: [ "$stock_item",  "$$order_item" ] },
                         { $gte: [ "$instock", "$$order_qty" ] }
                       ]
                    }
                 }
              },
              { $project: { stock_item: 0, _id: 0 } }
           ],
           as: "stockdata"
         }
    }
])
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/)

[Voltar para Sumário](#sumário)

---

### $addFields

**Template**

```
db.collection.aggregate([
  {
    $addFields: {
      <novoCampo1>: <valor> ,
      <novoCampo2>: <valor> ,
      ...
    },
  },
]);
```

**Exemplo**

```javascript
db.school.aggregate([
  {
    $addFields: {
      totalHomework: { $sum: "$homework" } ,
      totalQuiz: { $sum: "$quiz" }
    },
  },
  {
    $addFields: {
      totalScore: {
        $add: [ "$totalHomework", "$totalQuiz", "$extraCredit" ]
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/addFields/)

[Voltar para Sumário](#sumário)

## Operadores Aritméticos

### $add

**Template**

```
db.collection.aggregate([
  {
    $project: {
      <campo>: {
        $add: [ <expressão1>, <expressão2>, ... ] 
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.products.aggregate([
  {
    $project: {
      item: 1,
      total: {
        $add: ["$price", "$fee"] 
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/add/)

[Voltar para Sumário](#sumário)

---

### $subtract

**Template**

```
db.collection.aggregate([
  {
    $project: {
      <campo>: {
        $subtract: [
          <expression1>,
          <expression2>
        ]
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.products.aggregate([
  {
    $project: {
      item: 1,
      total: {
        $subtract: [
          { $add: ["$price", "$fee"] },
          "$discount"
        ]
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/subtract/)

[Voltar para Sumário](#sumário)

---

### $ceil

**Template**

```
db.collection.aggregate([
  {
    $project: {
      roundedNumber: {
        $ceil: <numero>,
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.movies.aggregate([
  {
    $project: {
      value: 1,
      ceilingValue: {
        $ceil: "$rating",
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/ceil/)

[Voltar para Sumário](#sumário)

---

### $floor

**Template**

```
db.collection.aggregate([
  {
    $project: {
      value: 1,
      roundedNumber: {
        $floor: <numero>,
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.movies.aggregate([
  {
    $project: {
      value: 1,
      floorValue: {
        $floor: "$value",
      },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/floor/)

[Voltar para Sumário](#sumário)

---

### $abs

**Template**

```
db.collection.aggregate([
  {
    project: {
      <campo>: {
        abs: <numero>,
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.operations.aggregate([
  {
    project: {
      delta: {
        abs: { $subtract: ["$start", "$end"] },
      },
    },
  },
]);
```
[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/abs/)

[Voltar para Sumário](#sumário)

---

### $multiply

**Template**

```
db.collection.aggregate([
  {
    project: {
      <campo>: {
        $multiply: [ <expressão1>, <expressão2>, ... ]
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.operations.aggregate([
  {
    project: {
      date: 1,
      item: 1,
      total: {
        $multiply: [
          "$price",
          "$quantity"
        ]
      },
    },
  },
]);
```
[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/multiply/)

[Voltar para Sumário](#sumário)

---

### $divide

**Template**

```
db.collection.aggregate([
  {
    project: {
      <campo>: {
        $divide: [ <expressão1>, <expressão2> ]
      },
    },
  },
]);
```

**Exemplo**

```javascript
db.employees.aggregate([
  {
    project: {
      name: 1,
      workdays: {
        $divide: ["$hours", 8]
      },
    },
  },
]);
```
[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/divide/)

[Voltar para Sumário](#sumário)

---

## Operadores Comparativos

### $lt

  - **Lower Than** - Seleciona documentos cujo valor do campo especificado é **menor que** o valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $lt: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.vendas.find([
  {
    {
      valor: { $lt: 100.00 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/lt/)

[Voltar para Sumário](#sumário)

---

### $lte

  - **Lower Than or Equal** - Seleciona documentos cujo valor do campo especificado é **menor ou igual que** o valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $lte: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.vendas.find([
  {
    {
      qtd: { $lte: 120 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/lte/)

[Voltar para Sumário](#sumário)

---

### $gt

  - **Greater Than** - Seleciona documentos cujo valor do campo especificado é **maior que** o valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $gt: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.vendas.find([
  {
    {
      valor: { $gt: 10.00 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/gt/)

[Voltar para Sumário](#sumário)

---

### $gte

  - **Greater Than or Equal** - Seleciona documentos cujo valor do campo especificado é **maior ou igual que** o valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $gte: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.vendas.find([
  {
    {
      qtd: { $gte: 10 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/gte/)

[Voltar para Sumário](#sumário)

---

### $eq

  - **Equal** - Seleciona documentos cujo valor do campo especificado é **igual** ao valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $eq: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.consultas.find([
  {
    {
      medico_id: { $eq: 174 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/eq/)

[Voltar para Sumário](#sumário)

---

### $ne

  - **Not Equal** - Seleciona documentos cujo valor do campo especificado **não é igual** ao valor especificado

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $ne: <valor> },
    },
  },
]);
```

**Exemplo**

```javascript
db.consultas.find([
  {
    {
      medico_id: { $ne: 127 },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/ne/)

[Voltar para Sumário](#sumário)

---

### $in

  - **value In** - Seleciona documentos cujo campo especificado **possui valor igual a qualquer valor presente no array** passado para o operador

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $in: [<valor1>, <valor2>, ...] },
    },
  },
]);
```

**Exemplo**

```javascript
db.consultas.find([
  {
    {
      doenca_id: { $in: [201, 202, 203, 204, 205] },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/in/)

[Voltar para Sumário](#sumário)

---

### $nin

  - **value Not In** - Seleciona documentos cujo campo especificado **possui valor diferente a qualquer valor presente no array** passado para o operador

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $nin: [<valor1>, <valor2>, ...] },
    },
  },
]);
```

**Exemplo**

```javascript
db.consultas.find([
  {
    {
      sintomas_id: { $nin: [171, 174, 180] },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/nin/)

[Voltar para Sumário](#sumário)

---

## Operadores Lógicos

### $not

  - **Not** - Executa uma **operação de negação (NÃO)** com o operador ou com a expressão passada como parâmetro.

**Template**

```
db.collection.find([
  {
    {
      <campo>: { $not: { <operador ou expressão> } },
    },
  },
]);
```

**Exemplo**

```javascript
db.carros.find([
  {
    {
      marca: { $not: 'Fiat' },
    },
  },
]);
```

```javascript
db.carros.find([
  {
    {
      valor: { $not: { $gt: 12000.00 } },
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/not/)

[Voltar para Sumário](#sumário)

---

### $or

  - **Or** - Executa uma **operação de disjunção (OU)** com os operadores ou as expressões passadas como parâmetros em um array, selecionando os documentos onde alguma das expressões seja verdadeira.

**Template**

```
db.collection.find([
  {
    {
      $or: [{ <operador ou expressão 1> }, { <operador ou expressão 2> }, ...],
    },
  },
]);
```

**Exemplo**

```javascript
db.carros.find([
  {
    {
      $or: [{ marca: 'Fiat' }, { marca: 'Ford' }],
    }
  },
]);
```

```javascript
db.carros.find([
  {
    {
      $or: [{ ano: { $lte: 1980 } }, { ano: 2000 }] ,
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/or/)

[Voltar para Sumário](#sumário)

---

### $nor

  - **Not ... Or ...** - Executa uma **operação de negação (NÃO) conjunta** com os operadores ou as expressões passadas como parâmetros em um array, selecionando os documentos onde todas as expressões falhem.

**Template**

```
db.collection.find([
  {
    {
      $nor: [{ <operador ou expressão 1> }, { <operador ou expressão 2> }, ...],
    },
  },
]);
```

**Exemplo**

```javascript
db.carros.find([
  {
    {
      $nor: [{ marca: 'Peugeot' }, { nome: 'Marea' }],
    }
  },
]);
```

```javascript
db.carros.find([
  {
    {
      $nor: [{ valor: { $gte: 50000.00 } }, { ano: { $lt: 1990 } }] ,
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/nor/)

[Voltar para Sumário](#sumário)

---

### $and

  - **And** - Executa uma **operação de conjunção (E)** com os operadores ou as expressões passadas como parâmetros em um array, selecionando os documentos onde todas as expressões sejam verdadeira. Caso alguma falhe, o MongoDB não avaliará as expressões restantes.

**Template**

```
db.collection.find([
  {
    {
      $and: [{ <operador ou expressão 1> }, { <operador ou expressão 2> }, ...],
    },
  },
]);
```

**Exemplo**

```javascript
db.cidades.find([
  {
    {
      $and: [{ litoral: true }, { regiao_id: 1 }],
    }
  },
]);
```

```javascript
db.cidades.find([
  {
    {
      $and: [{ populacao: { $gte: 1000000 } }, { regiao_id: { $in: [0, 1, 2] } }] ,
    },
  },
]);
```

[Documentação](https://docs.mongodb.com/manual/reference/operator/query/and/)

[Voltar para Sumário](#sumário)

---
