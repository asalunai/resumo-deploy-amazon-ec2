# Automatizando a Instância EC2

## Criando uma Instância automatizada

Vamos começar com uma imagem pronta e customiza-la.

📌 Esse processo vale para qualquer imagem de maquina. Você pode utilizar uma imagem do Marketplace, por exemplo: uma máquina otimizada para WordPress.

Criando um template customizado (ou imagem de máquina - AMI):
* Em "Launch Instance" => "Choose AMIs"
* Em "Quick Start": selecione Amazon Linux AMI (nosso padrão)
* Em "Configure Instance": habilite o "Terminate Protection"
* Ainda em "Configure Instance": Vá em "Advance Details" e cole o conteúdo do ![script shell](https://github.com/asalunai/resumo-deploy-amazon-ec2/blob/main/extras/script.sh)
* Em "Security Group": selecione o que criamos anteriormente: acesso-remoto
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

## Testando a instância e ajustando as regras de acesso

Verificando se a instancia foi criada corretamente:
* Marque a instancia criada (chamada de web-dev)
* Em "Action" => "Settings" => "Get System Log": Ele mostra o log da instalação. Nele, é possível verificar se deu algum erro.
* Conecte na máquina web-dev no terminal
* Teste se o Apache está rodando o banco de dados: “netstat –ltum”. 
* Confira as permissões: no diretório /var dê um “ls –l”. Verificar se o diretório /www está com dono ec2-user e grupo apache. 

Configuração do acesso Web à maquina web-dev:
* No Dashboard, vá em "Security Group"
* Crie uma regra nova em "Create Securuty Group"
    * nome e descrição: "acesso-web"
    * em "Inbound", adicione uma regra para HTTP e outra regra para HTTPS 
* Volte na instancia e selecione a maquina web-dev
* Vá em "Action" => "Networking" => "Change security group"
* Adicione a regra "acesso-web" à instancia
* Abra o navegador e acesse o IP público da maquina web-dev. Deve carregar a página de teste do Apache.

📌 Lembre-se de sempre adicionar um nome e uma descrição (geralmente o nome) em tudo que criar para não se perder depois!

