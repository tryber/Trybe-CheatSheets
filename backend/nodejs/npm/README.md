# NPM Comandos Cheat Sheet

# Sumário

- [Criar novo pacote Node.js](#criar-novo-pacote-nodejs)
  - [Cria o arquivo `package.json` personalizado](#cria-o-arquivo-packagejson-personalizado)
  - [Cria o arquivo `package.json` padrão](#cria-o-arquivo-packagejson-padrão)
- [Instalar pacotes no Node.js](#instalar-pacotes-no-nodejs)
  - [Instala **todas** as depedências do arquivo `package.json`](#instala-todas-as-depedências-do-arquivo-packagejson)
  - [Instala o **pacote desejado** nas depedências do arquivo `package.json`](#instala-o-pacote-desejado-nas-depedências-do-arquivo-packagejson)
  - [Instala o **pacote desejado** nas depedências de **desenvolvimento** do arquivo `package.json`](#instala-o-pacote-desejado-nas-depedências-de-desenvolvimento-do-arquivo-packagejson)
- [Remover pacotes no Node.js](#remover-pacotes-no-nodejs)
  - [Remove o **pacote desejado** das depedências do arquivo `package.json`](#remove-o-pacote-desejado-das-depedências-do-arquivo-packagejson)
- [Criar scripts no arquivo `package.json`](#criar-scripts-no-arquivo-packagejson)
  - [Cria script para iniciar a **aplicação principal**](#cria-script-para-iniciar-a-aplicação-principal)
  - [Cria **script personalizado** para o pacote desejado](#cria-script-personalizado-para-o-pacote-desejado)
- [Executar scripts do arquivo `package.json`](#executar-scripts-do-arquivo-packagejson)
  - [Executa a **aplicação principal** do pacote que esta criando](#executa-a-aplicação-principal-do-pacote-que-esta-criando)
  - [Executa o **script registrado** no arquivo `package.json`](#executa-o-script-registrado-no-arquivo-packagejson)

---

## Criar novo pacote Node.js

---

### Cria o arquivo `package.json` personalizado
```shell
$ npm init
```

### Cria o arquivo `package.json` padrão
```shell
$ npm init -y
```

[Voltar para Sumário](#sumário)

---

## Instalar pacotes no Node.js

---

### Instala **todas** as depedências do arquivo `package.json`
```shell
$ npm install
```

### Instala o **pacote desejado** nas depedências do arquivo `package.json`

**Template**
```shell
$ npm install <package_name>
```

**Exemplo**
```shell
$ npm install express
```

### Instala o **pacote desejado** nas depedências de **desenvolvimento** do arquivo `package.json`

**Template**
```shell
$ npm install <package-name> --save-dev
```
OU
```shell
$ npm install <package-name> -D
```

**Exemplo**
```shell
$ npm install jest --save-dev
```
OU
```shell
$ npm install jest -D
```

[Voltar para Sumário](#sumário)

---

## Remover pacotes no Node.js

---

### Remove o **pacote desejado** das depedências do arquivo `package.json`

**Template**
```shell
$ npm rm <package_name>
```

**Exemplo**
```shell
$ npm rm express
```

[Voltar para Sumário](#sumário)

---

## Criar scripts no arquivo `package.json`

---

### Cria script para iniciar a **aplicação principal**
**Template**
```json
{
  "scripts": {
    "start": "node nome_do_seu_arquivo.js"
  }
}
```

**Exemplo**
```json
{
  "scripts": {
    "start": "node index.js"
  }
}
```

### Cria **script personalizado** para o pacote desejado
**Template**
```json
{
  "scripts": {
    "nome_do_comando_do_script": "o_que_deseja_executar"
  }
}
```
**Exemplo**
```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

[Voltar para Sumário](#sumário)

---

## Executar scripts do arquivo `package.json`

---

### Executa a **aplicação principal** do pacote que esta criando

```shell
$ npm start
```

### Executa o **script registrado** no arquivo `package.json`

**Template**
```shell
$ npm run <package-name>
```

**Exemplo**
```shell
$ npm run lint
```

[Voltar para Sumário](#sumário)

---
