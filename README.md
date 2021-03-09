# MongoDB Aggregation Cheat Sheet

# Sumário

- [Operadores](#operadores)
  - [Operadores Aggregation](#operadores-aggregation)
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

---

# Operadores

## Operadores Aggregation

### $match

**Template**

```javascript
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

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/match/")

---

### $limit

**Template**

```javascript
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

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/limit/")

---

### $group

**Template**

```javascript
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

[Documentação](https://docs.mongodb.com/manual/reference/operator/aggregation/group/")

---

### $project

**Template**

```javascript
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

---

### $unwind

**Template**

```javascript
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

---

### $lookup

**Template**

```javascript
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

---
