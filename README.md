# Guia básico de como configurar o Jenkins para automatizar processos de deploy

## Primeiro, iremos configurar o ambiente. Usaremos 2 VPS com Linux Ubuntu: uma VPS rodará o Jenkins e a outra rodará um Agent.

### Vamos começar configurando a VPS

### Instale o Docker na suas duas VPS iremos precisar, Em caso de duvidas consulte >>  [LINK](https://docs.docker.com/engine/install/ubuntu/)

```yml
# Adicionar a chave GPG oficial do Docker:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Adicionar o repositório às fontes do Apt:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### Instale o Git na VPS que ira seu agente. Obs agente é onde rodara sua aplicação.
```yml
sudo apt install git
```

### Rode um container docker do Jenkins na VPS Mestre que é onde iremos configurar outras VPS  <br/>[LINK DE DONWLOAD](https://hub.docker.com/r/jenkins/jenkins)
```yml
docker pull jenkins/jenkins
```

### Rode a imagem na porta de sua preferencia irei rodar na 8085
```yml
docker run -dti -p 8085:8080  --name jenkins jenkins/jenkins
```







