# Configurando-dois-servidores-🌐 
Construir dois servidores apache que ambos rodem serviços distintos
Instalar em uma nova máquina virtual, para instalar um servidor apache e um servidor DNS
```
sudo apt-get update
sudo apt-get install apache2
sudo etc/init.d/apache2 status
```
Entrar no diretório padrão para arquivos da web.
```
cd /var/www
```
Criar o diretorio para o site UFRN, com o arquivo HTML
```
sudo mkdir -p ufrn/public_html
cd /ufrn/public_html
sudo touch index.html
sudo nano index.html
```
Criar uma página HTML com conteúdo simples
```
<html>
<head><title>olá seja bem vindo a UFRN</title></head>
<body>
  <center>
    <h1>Bem vindo ao site da UFRN</h1>
  </center>
</body>
</html>
```
##  Configurar o Virtual Host do Apache🔧
Cria um novo "site virtual" chamado ufrn.com que aponta para o diretório do site, fazendo uma cópia de um arquvo e editando.
```
cd /etc/apache2/sites-available/
sudo cp 000-default.conf ufrn.conf
sudo nano ufrn.conf
```
Conteúdo de _ufrn.conf_
```
ServerAdmin admin@ufrn.com
ServerName ufrn.com
ServerAlias www.ufrn.com
DocumentRoot /var/www/ufrn/public_html
```
Reiniciar o apache para ativar o serviço
```
sudo /etc/init.d/apache2 restart
```
Editar a configuração de rede cabeada colocando o ip escolhido 
IP:192.168.0.11 
Máscara:255.255.255.0 
Gateway: 192.168.0.1
DNS: 192.168.0.10
Desligar a máquina e colocar ela como rede interna, trocar de NAT para rede interna
## Na outra máquina que contém o servidor DNS📡 
Copia o modelo de zona de outro domínio (ifrn) e adapta para ufrn
```
cd /etc/bind/
sudo cp db.ifrn.com db.ufrn.com
sudo nano db.ufrn.com
```
Trocar todos os nomes e IPs para refletir ufrn.com e o IP 192.168.0.11
## Configurar outra zona direta📄
```
sudo nano named.conf.local
```
Colocar o conteúdo 
```
//zona direta UFRN
zone”ufrn.com” {
	type master;
	file “/etc/bind/db.ufrn.com”;
};
```
## Configurar a zona reversa🔁
Configurar a zona reversa em outro arquivo
```
sudo nano /etc/bind/db.reverso10
```
Adicionar a lihha:
```
11    IN    PTR    ufrn.com.
```
Reiniciar os serviços
```
sudo /etc/init.d/named restart
```
Desligar e colocar a máquina como rede interna
## Teste final 📶
Fazer o ping entre as máquinas
e acessar o site pelo navegador






