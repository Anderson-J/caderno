# Anotações

O containerizar é uma forma de isolar processos de modo que eles não afetem o sistema hospedeiro

Namespaces
Promovem o isolamento de processos

Cgroups
Promove o controle de recursos do hospedeiro

File System
Promove a determinação de camadas de sistema de arquivos

Imagens
Promovem a abstração de camadas interdependentes de modo que seja possível a modificação ou reutilização de qualquer uma dessas camadas

Dockerfile
É um arquivo declarativo que pode ser utilizado para definir a construção de imagens

Os containers possuem uma camada de leitura e escrtia que é volátil

As imagens ficam armazenadas em um "Image Registry"

A arquitetura do Docker é composta de:

- Docker Host
- Cache
- Gerenciador de volumes
- Daemon API
- Network

Inerente ao Docker existe ainda um client que é utilizado para acesso a "Daemon API" e o "Image Registry" de onde serão providas as imagens.

- Docker Client
- Image Registry

Um container geralmente termina ao finalizar a execução do seu processo, para que o cotnainer continue rodando é necessário que algum dos processos do container segure a execução de modo que ele não termine e saia, uma das maneira de fazer isso é utilizando o comando "sleep infinity" ex.:

```shell
docker run -d --rm ubuntu sleep infinity
```

no exemplo acima utilizamos também as opções "-d" e "--rm", elas servem para que o container não fique preso na sua tela e caso ele saia automaticamente seja removido.


Portas dos containers não necessáriamente estão esposta para o host, para que isso aconteça é necessário que a porta seja publicada para o host utilizando a opção "-p" de modo que indiquemos a portaDoHost:portaDoContainer ex.:

```shell
docker run -p 8080:80 nginx
```

No exemplo acima a porta 8080 correponde a porta 8080 do host, já a porta 80 é referente ao porta 80 do container nginx

Para persistencia de dados com Docker é necessário que seja feita a criação de volumes ou o apontamento direto do caminho onde os dados serão persistidos, uma vez que os containers não possuem camada de persistencia, se faz necessário que essa camada seja indicada de modo "externo" pois assim caso o container deixe de existir os dados persistirão, é possível fazer com a indicação de um caminho externo, por ex.:

```shell
docker run -d --rm --name nomeDaImagem -v caminhoDoHost:caminhoDoContainerParaPersistencia imagem
```

ou também indicando diretamente um volume, ex.:

```shell
docker run -d --rm -name nomeDaImagem -v nomeDoVolume:caminhoDoContainerParaPersistencia imagem
```

além disso, apra utilizar a forma de volumes se faz necessário criar o volume a ser utilizado, conforme ex.:

```shell
docker volume create nomeDoVolume
```

O docker utiliza imagens para criação dos containers, essas imagens ficam armazenadas em um repositório "Registry" que armazena as imagens, no caso do Docker o "Registry" padrão utilizado é o Docker Hub, porém é importante saber que é possível modificar o registry padrão.

é possível tanto baixar imagens de um registy quanto enviar imagens para um, para baixar imagens de um container registry é com o seguinte comando:

```shell
docker pull nomeDaImagem
```

ou enviar para o Docker Hub e para isso será necessário criar suas imagens, é possível fazer isso através do "Dockerfile" é importante saber alguns conceitos a respeito do Dockerfile:

- Todo Dockerfile começa a partir de alguma imagem, mesmo que essa imagem seja um "scratch"
- Todo Dockerfile é uma especie de "receita de bolo" para a criação de imagens
- Assim como uma receita de bolo é necessário que as instruções sejam executadas para que o bolo fique pronto, no caso do Dockerfile é necessário dar um "build" no Dockerfile.

segue um exemplo de um Dockerfile:

```Dockerfile
FROM ubuntu:latest

WORKDIR /home

RUN apt update && apt upgrade -y

COPY localDoQueQuerCopiar localDeDestinoDaCopia]

ENTRYPOINT ["echo", "Comando imutável que serpre será executado a risca, porém pode receber um CMD como parâmetro"]

CMD [" Comando mutável e substituível, que pode ser passado ou recebido como parâmetro para o ENTRYPOINT"]

ENV variavelDeAmbiente

EXPOSE 80

```

Uma vez que tenha o Dockerfile pronto, é possível dar "build" com o comando:

```shell
docker build -t nomeDeUsuarioDoDockerHub/nomeDaImagem:tag caminhoDoDockerfile
```

Para publicar a imagem comece logando no registry:

```shell
docker login

# Entre com suas credenciais de acesso ao Docker Hub
```

Após isso utilize o comando:

```shell
docker push nomeDeUsuarioDoDockerHub/nomeDaImagem:tag
```

A respeito da rede utilizada pelo Docker existem algumas que são consideradas as mais usadas, são elas:

- bridge (rede padrão - possui isolamento da rede do host, porém possui comunicação com outros containers, é importante saber que a rede bridge padrão que já vem criada no Docker não possui resolução de DNS)
- host (cria um compartilhamento com a rede do host)
- overlay (mais utilizado com o docker em modo swarm, permite sobrepor a rede do host)
- macvlan (faz um bind de um endereço mac a interface de rede)
- none (isola completamente o container da rede)

o comando padrão para criação de uma rede no Docker é:

```shell
docker network create nomeDaRede
#
# Com esse comando criará uma rede bridge por padrão, caso precise criar uma rede com um driver diferente como o "host" por exemplo pode utilizar o seguinte comando:
#
# docker network create --driver host nomeDaRede
```

```importante!
É possível que em algum momnento seja necessário acessar um recurso que esteja rodando nativamente no host, como um servidor web que esteja instalado diretamente na máquina por exemplo, nesse caso para que um container tenha acesso a uma porta localmente se faz necessário que ele realize esse acesso utilizando o endereço "host.docker.internal:porta"
```
