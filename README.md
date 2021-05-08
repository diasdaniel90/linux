# Linux
###Desafio 4Linux###
INICIADO AS 19:30
chmod 400 chave.pem 
ssh -i chave.pem suporte@<IP>
sudo apt update
sudo apt install nginx

#ADD a seguinte linha "34.68.238.255	4linux.local.com.br" em /etc/hosts no meu notebook

#INSTALL PRÉ-REQUISITOS
 sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
#CERTIFICADO DOCKERT PARA DOWNLOAD
 curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 #INCREMENTANDO INFORMAÇÕES DO REPOSITÓRIO
 echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
#INSTALANDO
sudo apt-get install docker-ce docker-ce-cli containerd.io
#HABILITANDO OS SERVICES
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
#ADD O USER suporte ao grupo docker para não precisar chamar o sudo
sudo usermod -aG docker suporte
#baixando as images a serem utilizadas.

docker pull wordpress
docker pull mariadb

#INSTALANDO O COMPOSE
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

#CRIANDO DIR PARA ARMAZENAR OS VOLUMES DO CONTAINER 
mkdir /home/suporte/docker
#CRIANDO VOLUMES
docker volume create 


#CRIADO O ARQUIVOS docker-compose.yml para o banco de dados foi criado um quarto container #VER O ARQUIVO "docker-compose.yml" no github
docker-compose up -d

#RESOLVER MSG DE ERRO DAS DEFINIÇÕES DE LOCALIDADE. 
sudo apt install locales
sudo apt-get install --reinstall locales && sudo dpkg-reconfigure locales

#nginx
sudo systemctl enable nginx
sudo systemctl restart nginx
sudo rm /etc/nginx/sites-enabled/default 
cd /etc/nginx/sites-available
sudo touch app1.4linux.local.com.br
sudo touch app2.4linux.local.com.br
sudo touch app3.4linux.local.com.br
#HABILITANDO OS SITES CRIADOS
sudo ln -s /etc/nginx/sites-available/app1.4linux.local.com.br /etc/nginx/sites-enabled/app1.4linux.local.com.br
sudo ln -s /etc/nginx/sites-available/app2.4linux.local.com.br /etc/nginx/sites-enabled/app2.4linux.local.com.br
sudo ln -s /etc/nginx/sites-available/app3.4linux.local.com.br /etc/nginx/sites-enabled/app3.4linux.local.com.br

sudo systemctl restart nginx
sudo cp app1.4linux.local.com.br app2.4linux.local.com.br 
sudo cp app1.4linux.local.com.br app3.4linux.local.com.br 
sudo systemctl restart nginx

docker ps

docker logs 0cb46317fd04 2>/dev/null | grep "GENERATED ROOT PASSWORD"


docker container run --name appdb -e MYSQL_ROOT_PASSWORD=alura

#CRIANDO OUTRAS DATABASES para o app2 e app3


docker exec -i -t appdb mysql -u root -ppassroot
CREATE DATABASE wordpressdb2;
GRANT ALL PRIVILEGES ON wordpressdb2.* TO 'wordpressuser'@'%';
FLUSH PRIVILEGES;

CREATE DATABASE wordpressdb3;
GRANT ALL PRIVILEGES ON wordpressdb3.* TO 'wordpressuser'@'%';
FLUSH PRIVILEGES;


#DOCKERFILE MARIADB#
FROM mariadb





