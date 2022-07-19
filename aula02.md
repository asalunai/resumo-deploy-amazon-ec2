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

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a02pt01img1.PNG?raw=true)

📌 Ver mais sobre regiões e zonas da AWS: https://aws.amazon.com/pt/about-aws/global-infrastructure/regions_az/

Se você clicar com o botão direito => state da instância, você pode Parar ou Terminar. 

	* Terminar: você destrói a máquina, ou seja, você vai excluir a máquina. 
	* Parar: simplesmente para a maquina. 
	
![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a02pt01img2.PNG?raw=true)

### Acesso Remoto

⚠️ Durante o curso, a estação que eu recomendo você usar para treinar é uma estação Linux. O Linux já tem um SSH nativo. 

Para eu não digitar uma linha de comando enorme, o que eu faço? Marcou a sua EC2, clica em connect. Ele já dá certinho aqui a linha de comando para nós fazermos o acesso. Eu vou copiar essa linha e vou fazer o seguinte. Abrir aqui o meu terminal, vou fazer aqui “cd LABS/” dentro do diretório LABS, que é o diretório de aula.

A minha chave SSH eu vou mover para esse diretório. 
Essa chave tem que ser de leitura só com o dono. Como é que você muda isso? “chmod 400”

“ssh -i”. O “i” é a é a chave de acesso. Eu botei a chave aqui para executar o comando aqui no mesmo diretório. Se estiver em outro diretório, bota o path da sua chave. 

Importante aqui: nome do usuário e o host remoto.
[05:08] Nome do usuário. Essa máquina Amazon Linux, por default, cria um usuário ec2-user. A máquina Ubuntu tem lá ubuntu e o nome do usuário. Cada uma tem a particularidade. Quando você clicar no connect ou na documentação da máquina você tem acesso a isso. Essa daqui é a linha de comando. Isso daqui, ele cria uma entrada no DNS automaticamente para nós com essa extensão. Deixa eu voltar para você ver. Public DNS ec2-54, que nada mais é, se você reparar, o IP com outros nomes na frente, 54-235-6-8.

[05:55] Esse IP é dinâmico. Como assim? Se eu parar a minha máquina e voltar com ela, esse IP vai ser alterado. Tem como fixar isso? Tem, nós vamos ver. 

[07:32] Conectei na máquina pelo IP externo. Ela tem esse IP interno aqui. E a máquina está prontinha, está rodando para nós trabalharmos. 

