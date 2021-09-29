# Docker Comandos Cheat Sheets

# Sumário


### Para utilizar os comandos docker o mesmo será necessario estar instalado.
Link da documentação para instalação: https://docs.docker.com/engine/install/

#### Todo o comando docker é iniciado com $ `docker` seguindo de uma comando e depois um parametro para definirmos o que vamos fazer com essa comando.
```
$ docker <comando> <parametro>
```
Exemplo:

```
$ docker container ls
```
Esse comando lista os containers ativos.

## Vamos conhecer algumas comandos do docker.

Aqui irei somente listar as comandos existentes no docker.


podemos usar parametros em qualquer comando, como por exemplo listar, inspecionar e etc...

```
$ docker image
```
```
$ docker container
```
```
$ docker network
```
## Parametros docker
```
ls
ls -a
```
comandos de listagem.
```
rm
rm -f
```
comandos de remoção.

Com esses parametros podemos combina-los com qualquer comando.

## Lista de comandos mais utilizados no docker


```
$ docker container ls
```
Lista todos containers ativos.
```
$ docker container ls -a
```
Lista todos containers ativos e inativos.
```
$ docker container ls -l
```
Lista o ultimo container.
```
$ docker container run <image>
```
O parametro `run` podemos por qualquer imagen para rodar em nosso container.

Exemplo:
```
$ docker container run hello-world
```
Roda a imagem `hello-world` em nosso container.

```
$ docker container run -it ubuntu
```
* O comando -it são dois comandos juntos:

    * -t que indica pro docker que estamos requisitando um terminal no container que consiga imprimir o retorno dos nossos comandos;
    * -i que estabelece uma interface de comunicação física com esse terminal, no caso, por meio do teclado.
```
$ docker container run -d -it ubuntu
```
O parametro `-d` é para rodar o container em background
```
$ docker container run --name meu-container -d -it ubuntu
```
O parametro --name nos permite adicionar um nome ao nosso container
```
$ docker container top meu-container
```
O parametro `top` nos traz informações sobre o container.
```
$ docker container attach <CONTAINER ID || NAMES>
```
O parametro attach nos permite acessar um container que esteja em background.

Exemplo:
```
$ docker container attach meu-container
```


---
### Remover containers

```
$ docker container prune
```
o parametro `prune` remove todos os container inativos.
```
$ docker container rm  <CONTAINER ID || NAMES>
```
Removendo um container especifico.
Exemplo 
```
$ docker container meu-container
```
---
### Remover imagens

```
$ docker rmi -f <IMAGE ID>
```
Remove uma imagem em especifico.

```
$ docker image prune
```
Remove imagens que não tenham um container espelhado.

```
$ docker image prune -a
```
Remove todas as imagens mesmo que tenham um container espelhado.

* OBS: Ficara a alma da imagem, para ela ser removida completamente você irá precisar parar o container que está com a imagem rodando.

---
### Comando para baixar imagens

As imagens são baixadas por padrão pelo docker quando vamos rodar um container e não possuimos a imagem.

Mas caso você queira baixar somente a imagem sem rodar em um container podemos usar o comando:
```
$ docker pull <image>:<tag>
```
O parametro `tag` é opcional.
Exemplos:
```
$ docker pull alpine

$ docker pull alpine:3.13
```
---
### Comandos de Rede

```
$ docker  run -d -P httpd:2.4
```
o parametro `-P` é utilizado para que o docker faça um mapeamento de portas automático, você pode verificar a porta listando o container(vai estar na propriedade `PORT`).
```
$ docker run -d -p 54321:80 httpd:2.4
```
O parametro `-p` é usado quando vamos mapear a porta manualmente.

### Comandos de volume

```
docker run -d --name site-trybe -p 8881:80 -v <Meu-diretorio>:<container-diretorio> httpd:2.4
```
O parametro `-v` é para definirmos um volume para nosso container, onde a primeira entrada é o local de nossa maquina e a segunda entrada é local do container.
Exemplo:
```
docker run -d --name site-trybe -p 8881:80 -v "/home/trybe/meu-site/:/usr/local/apache2/htdocs/" httpd:2.4
```
Locais de volumes dos containers podem ser encontrados em suas documentações.

---
### Comandos para parar container em background
```
$ docker stop <container id || names>
```
O comando `stop` para o container.
Exemplo:
```
$ docker stop site-trybe
```
---
### Comandos depreciados

```
$ docker ps
```
Lista containers ativos.
```
$ docker ps -a
```
Lista containers ativos e inativos.
```
$ docker images
```
Lista todas as imagens.

---

### Comandos Dockerfile

