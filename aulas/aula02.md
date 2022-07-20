# Gerenciando Instâncias EC2

## Informações e acesso remoto

Vamos visualizar a instancia que acabamos de criar.

### Informações

Quando você dá o view, ele volta aqui para instância e mostra algumas informações e o status dela aqui. 

Primeiro passo, antes de nós explorarmos, é colocar um nome. Vou botar aqui a “primeira-ec2”. O nome dá uma referência para nos facilitar.

Tem algumas coisas importantes:

* O ID da máquina. 
* tipo de maquina: t2.micro. 
* Aonde ela está provisionada? us-east1-1d.
* O IP de acesso, o IP público de acesso.

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/imagens/a02pt01img1.PNG?raw=true)

📌 Ver mais sobre regiões e zonas da AWS: https://aws.amazon.com/pt/about-aws/global-infrastructure/regions_az/

Se você clicar com o botão direito => state da instância, você pode Parar ou Terminar. 

	* Terminar: você destrói a máquina, ou seja, você vai excluir a máquina. 
	* Parar: simplesmente para a maquina. 
	
![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/imagens/a02pt01img2.PNG?raw=true)

### Acesso Remoto

⚠️ Durante o curso, a estação que eu recomendo você usar para treinar é uma estação Linux. O Linux já tem um SSH nativo. 

Para facilitar e manter a organização do curso, crie um diretorio dentro da sua maquina. Aqui, será “LABS/”.

Conexão SSH:
* Marque a sua EC2 e clica em "Connect" na barra superior do painel. 
* Copie a linha de comando no painel que vai abrir. 
* Abra o terminal do Linux da sua maquina local e entre no diretorio “LABS/”
* Mova a chave SSH para esse diretório.
* Essa chave tem que ser de leitura só com o dono. Utilize “chmod 400”, Ex. chmod 400 aws-ricardo.pem
* Execute o comando.
* Utilize o IP público no navegador para testara conexão.

Explicando o comando: 

* ssh -i "aws-ricardo.pem" ec2-user@ec2-54-236-6-68.compute-1.amazonaws.com
	* -i : insere a chave ssh
	* "aws-ricardo.pem" : a chave ssh. se enstiver em outro diretório, colocar a path completa.
	* ec2-user@ec2-54-236-6-68.compute-1.amazonaws.com : DNS público

⚠️ Esse IP (DNS público) é dinâmico. Se eu parar a minha máquina e voltar com ela, esse IP vai ser alterado. Veremos isso com mais detalhes ao longo do curso. 

### Proteção contra exclusão 

Boas práticas: Quando trabalhamos com maquinas em produção, é prudente configurar a proteção conta exclusão para evitar acidentes.

* Clique em "Actions" na barra superior do Dashboard
* Em "Configuração da Instancia", clique em "Change Termination Protection"
* Na janela que abrir, clique em Habilitar.
* Para desabilitar, repita o processo e no final clique em Desabilitar 

### Comunicação das instâncias: Associando security groups às instâncias

Agora vamos criar agora uma terceira e ver como é que essas máquinas se comunicam, porque no seu dia a dia você vai ter várias máquinas. 

Essas máquinas são criadas dentro de uma mesma sub-rede. Como é que fica a comunicação entre elas? 

Como essa máquina é baseada em Red Hat, a recomendação é: conectou a primeira vez, faz lá: “sudo yum update”. Atualiza os pacotes dela.

Configurando a comunicação:
* Vá em: Action, Networking, Change Security Group.
* Adicione o grupo Default
* Faça isso com todas as máquinas envolvidas

📌 Default VPC security group: VPC, para quem ainda não sabe, é a questão das redes dentro da AWS. A Alura tem um curso só sobre VPC, com vários recursos para você trabalhar.
