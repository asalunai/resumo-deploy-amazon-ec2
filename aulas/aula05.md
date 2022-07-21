# Banco de Dados no Amazon RDS

## Instância para o banco de dados

Via de regra, os projetos apresentam a necessidade de segmentar a instância de aplicação da instância do banco de dados. 
E para fazer essa separação, nós vamos usar outro serviço da AWS chamado RDS. 
O RDS é uma instância preparada e otimizada para banco de dados. 

Criando um banco de dados RDS:

* No menu lateral, vá em RDS
* No Dashboard, clique em "Create Database"
* No nosso exemplo, vamos selecionar "Standart Create", "MySQL", "Template Free Tier"
* Em "Settings", Defina o nome da instância (ex. database-1), 
* Ainda em "Settings" defina um login (padrão: admin) e senha para o BD
* Por enquanto, deixaremos as demais configurações como padrão sugerido pela Amazon

📌 No processo de criação do DB, na parte de "Configurações Adicionais", note a opção de "Backups Automáticos". Ele gera snapshots de tempos em tempos. 

⚠️ Se atente a qual região a instancia está sendo criada. Se estiver tudo em uma mesma região, fica mais fácil de configurar.

## Criando o banco de dados

📌 VPC é a rede interna da Amazon.

📌 Uma VPC para o BD foi criada automaticamente dentro da região em que a instância do BD foi criada.
Essa VPC é a mesma das instâncias EC2 que criamos anteriormente, pois criamos tudo na mesma região.
Ou seja, todas as máquinas estão na mesma rede interna. 

⚠️ Se as instâncias foram criadas em regiões diferentes, será necessário alterar as regras de comunicação lá no "Security Group".

Conectando no BD:

* No Dashboard do BD, vá em "Conectivity and Security"
* A referência de conexão do BD é o campo "Endpoint" e "Port"
* O Endereço é o Endpoit e a porta de conexão é a 3306
* No menu lateral, volte em "Instâncias"
* Na máquina web-dev criada anteriormente, vá em "Secutity Group" e habilite a conexão VPC
* No termial Linux, conecte na maquina web-dev. ⚠️ Na linha de comando que a AWS gera automaticamente, o acesso estará em nome do usuário "root". Isso acontece porque a instancia foi gerada através de um template. Substitua "root" pelo "ec2-user" para evitar erros.
* Utilize o comando  “mysql –u admin –h <endpoint> -p” para conectar no BD utilizando a senha do banco. 

Vamos criar uma tabela para testes. É uma aplicação simples de inserção earmazenamento de nome e e-mail.
	
Criando uma tabela teste:

* Conecte ao banco
* insira o comando “show databases;”
* insira o comando “create database cadastro;”
