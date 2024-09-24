# Procetimento de instalação do Docker

Atualize o sistema:

```shell
sudo apt update && sudo apt upgrade -y
```

Após isso garanta que não há nenhuma possível instalação antiga do docker

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Adicione o repositório apt:

```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

o comando abaixo instala a última versão disponível do docker:

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Caso deseje instalar uma versão específica do Docker comece listando as versões disponíveis:

```shell
# List the available versions:
apt-cache madison docker-ce | awk '{ print $3 }'
```

escolha a versão pretendida e instale com o comando:

```shell
VERSION_STRING=VersaoPretentida
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

Verifique se a instalação foi bem sucedida utilizando o comando:

```shell
docker --version
```
