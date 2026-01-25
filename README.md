# Bitwarden
Este repositório documenta a instalação e configuração do servidor self-hosted do Bitwarden.
Bitwarden é um software de armazenamento de senhas.

# Instalação

Primeiramente, neste repositório estaremos instalando o Bitwarden no Ubuntu Server. Para um guia de como instalar o Ubuntu Server: 

```
https://youtu.be/uUUCLoDyIx8?si=HyztHFpD30TIMJpZ
```

## O bitwarden será implantando utilizando docker, enão para instalar o docker:

```
  sudo install -m 0755 -d /etc/apt/keyrings
	sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
	sudo chmod a+r /etc/apt/keyrings/docker.asc
	echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	sudo apt update
	sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Criar um usuário que inicializará e utilizará o bitwarden, e seu diretório

Vale destacar que o usuario não pode ter permissão root, como descrito na documentação do Bitwarden, justificando assim sua criação.

Para criar o usuário, seu diretório e adicionar ao grupo docker:

```
  sudo adduser bitwarden_usr
	sudo addgroup docker
	sudo adduser bitwarden_usr docker
	sudo mkdir /opt/bitwarden_usr
	sudo chmod -R 700 /opt/bitwarden_usr
	sudo chown -R bitwarden_usr:bitwarden_usr /opt/bitwarden_usr
```
## Instalar o Bitwarden

Primeiramente, é necessário definir um domínio para o Bitwarden, seja ele local ou público.

Neste repositório, utilizaremos um domínio local, criado por meio do meu servidor DNS AdGuard.
(Caso você tenha acesso ao roteador, também é possível reservar o domínio diretamente nele.)

O domínio utilizado neste guia será: bitwarden.local.

Tendo o domínio definido, seguimos com a instalação:

```
  su bitwarden_usr
	cd /opt/bitwarden_usr
	curl -Lso bitwarden.sh "https://func.bitwarden.com/api/dl/?app=self-host&platform=linux" && chmod 700 bitwarden.sh
	./bitwarden.sh install
```
após isso, aparecerá o menu de instalação, onde você terá que responder as seguintes questões (irei responder com o meu caso em específico):

Nome do dominio: ´bitwarden.local´
Deseja que seja criado um certificado Let´s encrypt para o seu dominio? ´n´ (pois como é local, iremos configurar manualmente)
Nome do banco de dados: ´secretname´
Indicar o id de instalação: Nesse passo entraremos no endereço indicado e apontaremos nosso e-mail e a região (US), para assim gerar o id. 
Indicar a key da instalação: gerada no passo anterior
Região: US
Gerar um certificado ssl autoassinado: ´y´

A partir disso o bitwarden será instalado. 
Para iniciar basta estar logado no usuário bitwarden_usr e digitar os seguintes comandos:

```
- cd /opt/bitwarden_usr
- ./bitwarden.sh start
```

e para acessar basta digitar no navegador: bitwarden.local.

