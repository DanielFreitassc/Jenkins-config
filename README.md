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
### Agora va no nevegadore procure pelo Hostname da maquina exemplo : http://192.168.0.1:8085/
obs coloque a porta que vc escolheu no nosso caso foi 8085

### Agora vc precisara ir na VPS que esta rodando o jenkins e rodar o comando
```yml
docker ps
```
### copie o Container ID e digite 
```yml
docker logs [container ID]
```
copie a senha que aparecera é e cole no navegador

# Instalaremos alguns plugins 
- SSH Agent Plugin
                          
          
# Agora iremos em configurações siga os passos abaixo 

### Clique emGerenciar Jenkins      
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/6d645856-a7c3-48e4-a8af-658575a31cd4)
                                                                        
### Clique em Nodes

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/99266ea9-baec-43ea-a7ae-b15527318b94)
### Clique em adicionar um novo nó
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/36a3ecf5-550c-4389-8ca7-9bd0d0809231)

### Ponha um nome para o nó e defina como Agente permanente

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/1452a1f3-223d-4aa2-9781-a75f1380cd1f)

## Configure o nó
## Opcional: Coloque uma descrição
## Número de executores: 1
## Diretório raiz remoto: esse diretorio sera onde estara o usuario da sua VPS que vai o agente  normamente é /home/jenkins
## Rótulos: É um auxilio para você poder chamar esse nó especifico coloque um rótulo que remeta a sua VPS que ira o agente
## Uso: Usar esse  nó o máximo possível. 
## Método de lançamento: Lauch agente Via SSH 
## Host: Ip ou hostname da sua maquina exemplo 192.168.0.1 
obs para conseguir o host é só ir na vps que rodara seu codigo e digite
```yml
hostname -i
```
## agora iremos criar um usuario jenkins e nossa credencial para acessar a VPS que será via SSH 
## Primeiro crie um usuário para o jenkins 
```yml
sudo adduser jenkins
```
## Adicione a permissão do jenkins para que ele possar usar docker sem sudo 
```yml
sudo usermod -aG docker jenkins
```
## Agora reinicie sua VPS para aplicar a permissão dada ao jenkins 

## Apos sua vps reiniciar logue com seu usuário jenkins que acabamos de criar ou logue como root e troque para o jenkins

```yml
su - jenkins
```
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/a201afdf-7a81-483b-b9cd-e4635b9f290f)

## Acesse sua VPS onde ira o agente e crie uma chave SSH com o comando abaixo 


![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/7821ed7d-c597-4549-b3f1-e4660264a37d)



                                      
  escrevendo ...                        
        
        
                                                                      
                
                  
          
        
