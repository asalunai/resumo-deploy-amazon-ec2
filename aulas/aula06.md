# Insfraestrutura para alta disponibilidade

## Preparando para o Auto Scaling

Precisamos agora, a partir desse nosso último template, gerar uma nova imagem. 
A minha instância, a partir de agora, não precisa mais ter uma base local, já que a ideia é separar.

* Acesse a máquina web-dev
* Execute o comando “sudo systemctl stop mariadb”. Ele vai parar o banco.
* Execute o comando “sudo systemctl desable mariadb”. Evita que inicialize automaticamente.
* vá no diretório www: "cd /var/www"
* crie um diretorio cadastro: "mkdir cadastro"
* dentro do diretorio /var/www/cadastro, cole o arquivo dbinfo.cadastro
* dentro do diretorio /var/www/html, cole o arquivo index.php
* Teste a instância acessando o IP público e veja se carrega a página de inserção de dados.
* só pra manter o padrão, volte no diretorio /var e execute “chown –R ec2-user:apache www/” 

📌 Ver exemplo da AWS sobre montagem de cenário.

⚠️ Os aquivos dbinfo.cadastro e index.php são generalizados. Então modifique-os com as informações da sua instancia e do seu BD!

## Imagem final para o ambiente de produção

Criando uma imagem de máquina:

* Pause a máquina (stop)
* Crie a imagem (Action => instância => Image => criar imagem) (chamaremos ela de web-template)
* Cria uma instancia baseada nessa imagem. Lmebre-se de habilitar no Security Group: acesso-remoto, acesso-web e VPC
* Teste a aplicação acessando o IP público da intancia e veja se carrega a página de inserção de dados.
