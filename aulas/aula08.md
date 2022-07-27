# AWS Command Line Interface

## Instalando a AWS CLI

Recomendo seguir as instruções atualizadas do site oficial: 

<https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html>

Depois de instalado, é importante cria um usuário exclusivo para interações via CLI:

* Na pagina do Console da AWS, vá em IAM
* No menu lateral, vá em "Users"
* Clique em "Criar Usuário"
* Defina um nome de usuário e define o acesso como "Programmatic Access". Avançar.
* Na página seguinte, crie um grupo de acesso. Defina um nome (admin) e escolhe a política "Acesso Administrativo" 
* Vá avançando e, na parte de Review, lembre-se de fazer o download do arquivo .csv! Essa é a chave e senha de acesso ao usuário que estamos criando

⚠️ O grupo que nós criamos tem uma política anexada. 
Escolhemos a política administrator access, que dá direito a todos os serviços da AWS. 
Você pode criar uma política, por exemplo, para usar só recurso da EC2. 
Outra só para o seu administrador de banco de dados. Etc...

Agora vamos configurar o acesso desse usuário lá na CLI:

* Volte no terminal do seu computador
* Execute "aws configure"
* Ele vai pedir um "AWS Access Key ID" e um "AWS Secret Access Key"
* Abra o CSV que baixamos e pegue as informaçoes necessárias
* Em "Default Region Name" defina a região que voce está trabalhando. No nosso caso, Norte da Virgínia é "us-east-1"
* Em "Default output format" defina como "json"

## Utilizando a AWS CLI com os serviços EC2



## Controlando a instância com a AWS CLI


