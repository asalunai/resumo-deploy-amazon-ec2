# Automatizando a Instância EC2

## Criando uma Instância automatizada

Vamos começar com uma imagem pronta e customiza-la.

📌 Esse processo vale para qualquer imagem de maquina. Você pode utilizar uma imagem do Marketplace, por exemplo: uma máquina otimizada para WordPress.

Criando um template customizado (ou imagem de máquina - AMI):
* Em "Launch Instance" => "Choose AMIs"
* Em "Quick Start": selecione Amazon Linux AMI (nosso padrão)
* Em "Configure Instance": habilite o "Terminate Protection"
* Em "Advance Detais": cole o conteúdo do ![script shell](https://github.com/asalunai/resumo-deploy-amazon-ec2/blob/main/extras/script.sh)
* Em Security Group: acesso-remoto
* Nos demais, segue o padrão sugerido automaticamente e crie a instância

No exemplo prático: o meu desenvolver pediu para que eu configurasse uma máquina com Apache, PHP e o banco de dados MariaBD. 

Detalhes do ![script shell](https://github.com/asalunai/resumo-deploy-amazon-ec2/blob/main/extras/script.sh):
* yum update -y -> Atualiza os pacotes
* amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2 -> Instala MariaDB e PHP
* yum install -y httpd mariadb-server -> Instala Apache e o MySQL
* systemctl start httpd -> Inicia Apache
* systemctl enable httpd -> Sobe o Apache
* systemctl start mariadb -> Inicia MariaDB
* systemctl enable mariadb -> Sobe MariaDB
* usermod -a -G apache ec2-user -> Adiciona user ec2-user no grupo Apache
* chown -R ec2-user:apache /var/www -> ajusta a permissão de tudo que estiver dentro do diretório /var/www para o usuário ec2-user e para o grupo apache.

