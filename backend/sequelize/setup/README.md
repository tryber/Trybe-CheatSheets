# Sequelize Setup Cheat Sheet

# Sumário

- [Sequelize Setup Cheat Sheet](#sequelize-setup-cheat-sheet)
- [Sumário](#sumário)
- [Guia](#guia)
  - [Instalação das dependências](#instalação-das-dependências)
    - [Instalação do sequelize](#instalação-do-sequelize)
    - [Instalação do pacote de comandos CLI](#instalação-do-pacote-de-comandos-cli)
    - [Instalação do `mysql2`](#instalação-do-mysql2)
  - [Inicialização de um projeto sequelize](#inicialização-de-um-projeto-sequelize)
  - [Criação de um arquivo _model_](#criação-de-um-arquivo-model)
  - [Arquivos _migration_](#arquivos-migration)
    - [Criação de um arquivo _migration_](#criação-de-um-arquivo-migration)
    - [Execução dos arquivos _migration_](#execução-dos-arquivos-migration)
    - [Reverter a _migration_](#reverter-a-migration)
    - [Reverter até uma _migration_ específica](#reverter-até-uma-migration-específica)
  - [Arquivos _seeds_](#arquivos-seeds)
    - [Criação de um arquivo _seed_](#criação-de-um-arquivo-seed)
    - [Execução dos arquivos _seeds_](#execução-dos-arquivos-seeds)
    - [Reversão dos arquivos _seeds_](#reversão-dos-arquivos-seeds)
      - [Reverter todos arquivos _seeds_](#reverter-todos-arquivos-seeds)
      - [Reverter _seed_ mais recente](#reverter-seed-mais-recente)
      - [Reverter _seed_ específica](#reverter-seed-específica)

---

# Guia

## Instalação das dependências

### Instalação do sequelize

```bash
$ npm install sequelize
```

[Voltar para Sumário](#sumário)

---

### Instalação do pacote de comandos CLI

```bash
$ npm install --save-dev sequelize-cli
```

[Voltar para Sumário](#sumário)

---

### Instalação do `mysql2`

```bash
$ npm install mysql2
```

[Voltar para Sumário](#sumário)

---

## Inicialização de um projeto sequelize

> O comando criará as pastas models, migrations, seeders e config.

```bash
$ npx sequelize-cli init
```

[Voltar para Sumário](#sumário)

---

## Criação de um arquivo _model_ 

> O comando irá gerar o arquivo model e o arquivo migration correspondente.

```bash
$ npx sequelize model:generate --name NomeDoModel --attributes nomeDoAtributo:string
```

[Voltar para Sumário](#sumário)

---

## Arquivos _migration_ 

### Criação de um arquivo _migration_ 

```bash
$ npx sequelize migration:generate --name migrationName
```

[Voltar para Sumário](#sumário)

---

### Execução dos arquivos _migration_ 

```bash
$ npx sequelize db:migrate
```

[Voltar para Sumário](#sumário)

---

### Reverter a _migration_

```bash
$ npx sequelize db:migrate:undo
```

[Voltar para Sumário](#sumário)

---

### Reverter até uma _migration_ específica

```bash
$ npx sequelize-cli db:migrate:undo:all --to XXXXXXXXXXXXXX-create-posts.js
```

[Voltar para Sumário](#sumário)

---

## Arquivos _seeds_ 

### Criação de um arquivo _seed_

```bash
$ npx sequelize seed:generate --name seedName
```

[Voltar para Sumário](#sumário)

---

### Execução dos arquivos _seeds_

> O comando executa **todos** os arquivos seeds

```bash
$ npx sequelize db:seed:all
```

[Voltar para Sumário](#sumário)

---

### Reversão dos arquivos _seeds_ 

#### Reverter todos arquivos _seeds_ 

```bash
$ npx sequelize db:seed:undo:all
```

[Voltar para Sumário](#sumário)

---

#### Reverter _seed_ mais recente 

```bash
$ npx sequelize-cli db:seed:undo
```

[Voltar para Sumário](#sumário)

---

#### Reverter _seed_ específica 

```bash
$ npx sequelize-cli db:seed:undo --seed name-of-seed-as-in-data
```

[Voltar para Sumário](#sumário)

---
