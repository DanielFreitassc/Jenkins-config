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
## agora iremos configurar o abiente do agente e criar um usuario jenkins e nossa credencial para acessar a VPS que será via SSH 
## Primeiro instale o java para o agente do jenkins poder rodar [LINK](https://www.oracle.com/br/java/technologies/downloads/) 
```yml
wget https://download.oracle.com/java/22/latest/jdk-22_linux-x64_bin.deb 
```
## Agora instale o java com o comando abaixo
```yml
dpkg -i jdk-22_linux-x64_bin.deb
```
## Verifique se o java foi instalado
```yml
java --version
```
## Agora crie um usuário para o jenkins 
```yml
sudo adduser jenkins
```
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/a201afdf-7a81-483b-b9cd-e4635b9f290f)

## Adicione a permissão do jenkins para que ele possar usar docker sem sudo 
```yml
sudo usermod -aG docker jenkins
```
## Agora reinicie sua VPS para aplicar a permissão dada ao jenkins 

## Apos sua vps reiniciar logue com seu usuário jenkins que acabamos de criar ou logue como root e troque para o jenkins

```yml
su - jenkins
```
## Agora iremos criar um chave SSH para acessar a VPS 
```yml
ssh-keygen -t rsa -b 4096 -C "jenkins"
```
de enter para salvar a chave no direotiro padrão ./ssh
insira uma senha ou deixe em branco dando enter 

## Entre no diretorio onde foi salvo a chave SSH
```yml
cd .ssh/
```

## Autorize a chave OBS: Tem que estar no diretorio com a chave
```yml
cat id_rsa.pub >> authorized_keys
```
## Copie a chave privada iremos voltar para a configuração no painel do jenkins
```yml
cat id_rsa
```


## No jekins na parte de Credentials clique em Add e escolha o jenkins

## Escolha SSH username whith private key
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/790d270c-f233-4407-81a8-fa814de0d3f0)

# Agora coloque um ID para indentificar sua chave  e um descrição tbm 
# O username é o que criamos jenkins
# Private key é aquela que pedimos pra você copiar >> cat id_rsa
# Passphrase coloque sua senha ou deixe em branco caso sua chave ssh n tenha senha de acesso


![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/e66c58a5-5f65-4490-b888-135e0754a3b6)

# Em Host Key Verification Strategy pode colocar Non verification Strategy e salvar 
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/b135985c-ece0-4bff-84be-807a8a405508)

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/7821ed7d-c597-4549-b3f1-e4660264a37d)

# Agora iremos configurar uma tarefa
# No painel de controler do jenkins escolhar nova tarefa

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/2d7942d8-f296-45b2-8ff3-ed2e9688a36f)
# Insira um nome para a tarefa e escolha software de estilo livre

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/68780c67-4ebd-4dca-8563-31581f54e6c6)

# Insira um descriçãpo e coloque a url do navegado que leva até o seu repositorio
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/cd5bf63f-d8e0-40d2-8a1c-34fa1a3749a6)


# Abaixo marque a opção Restringe onde este projeto pode ser executado

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/8a6b122a-e29c-45d5-a00a-dda6b163657e)

# Agora em Gerenciamento de código fonte marque o git como opção
# Coloque o link para clonagem do seu repositoro se ele for public não precisa inserir senha se ele necessitar de senha você tera que ir até o github e gerar uma chave gpc
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/337c5aba-b721-47ff-9eb8-2f31dbd8a2e6)

# Agora crie uma nova chave username é o nome do seu git e a senha é a chave gpc que vc criou no github lembre de colocar um id e um rotulo pra indentificar que chave é esta que você criou

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/cdba202a-3cec-4d40-a7bd-e53de166d066)


# Defina a branch em que sera feito um scanner procurando atualizações no nosso caso será a */main


![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/8e7c0451-547f-4d8c-a58e-6b2250896fae)


# Em Gatilho de disparo para construções marque >> Consultar periodicamente o SCM 
# Insira os caracteres abaixo para pedir para verificar a cada 1 minuto
```yml
* * * * *
```
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/cc1613bc-4613-414f-84a1-c426a39fe672)

# Agora em Ambiente de construção marque a opção SSH Agent e escolha a chave ssh que criamos anteriormente para o nó

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/060ed7e4-a947-4aa3-ba7e-8c35d3db2175)

# Em Passos de construção  adicone uma passso Executar shell
# Agora você pode passar uma lista de comando esritas em shell para que a cada nova atualização no github na branch main ele execute esse shell 
# coloque o comando abaixo e salva para testar
```yml
docker ps
```
# Clique no build feito
![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/96b0035d-a857-49ef-b144-f7da1b0e0e56)

# Clique em saida do console

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/f6fbe183-2526-456c-9d49-01d5fbbd343b)



A saida do console dever sera algo assim

![image](https://github.com/DanielFreitassc/Jenkins-config/assets/129224303/0dfd9acd-326e-4df1-908b-b50214e9086a)

E pronto configurações basicas feitas  basta colocar os comandos que você ja faz para rodar eles de forma automatica toda vez que você commitrar algo no github


