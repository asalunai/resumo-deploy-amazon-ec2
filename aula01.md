# Aula 01 - Primeiros passos na AWS

## Criando uma conta

Link AWS Amazon: aws.amazon.com

Procurar o botão de Criar conta e preenchero formulário.

:pushpin: Nota: É obrigatório cadastrar um cartão de crédito internacional.

:pushpin: Nota: O processo de ativação de conta pode demorar um pouco.  

## Uso gratuito e recomendações inicias

### Free Tier

Sobre o Free Tier: https://aws.amazon.com/pt/free/

Sobre preços: https://aws.amazon.com/pt/pricing/

Sobre caluladora de serviços: https://calculator.aws/#/

A AWS tem alguns serviçoes gratuitos. Eles são divididos em 3 tipos:

  1. 12 meses gratuitos: Quando você cria a sua conta, você ganha 12 meses de uso gratuito. Quando o período de vigência de 12 meses de uso gratuito expirar, ou se a utilização da aplicação ultrapassar os limites do nível gratuito, você pagará taxas de serviço padrão conforme o uso.
      * Instâncias EC2:  t2 micro Linux, ligada por até 750 horas ( ~1 mês inteiro)
      * Armazenamento S3: RDS
  2. Sempre Gratuito: É para sempre. Mesmo passados os 12 meses, vai continuar gratuito contanto que respeite os limites de uso. 
      * O Lambda é um FaaS (Function as a Service): É sempre gratuito até o limite de um milhão de chamadas, ocupando 128Mb de memória, e durando a cada segundo.
  3. Testes: Por exemplo: saiu um serviço novo. Ele te dá um período de teste gratuito naquele serviço. Passado o limite de uso, você vai ser cobrado as horas adicionais.
      * Machine Learning: Você tem 250 horas desse serviço. E quando foi estudar outro provedor, pegar o equivalente.

:warning: ATENÇÃO: Sempre leia o regulamento e as letras miúdas para depois não levar susto no cartão de credito!

### Recomendações iniciais

:pencil2: Inserir ilustrações!

:question: Como é que nós acessamos o console? Como nós usamos o dashboard da AWS? 

Você acabou de criar a sua conta, independente de onde você estiver, pode botar “AWS console”.

:warning: Recomendação de segurança! É opcional, mas fortemente recomendado.

Assim que criar a conta: na barra de busca, digita “iam”. 
E você ativa o segundo fator de autenticação: “Activate MFA on root account”. 

Recomendado: Token via o Google Authenticator.
Como é que você vincula o Google Authenticator à sua conta na AWS? 

Você vem em gerenciar MFA, marca a opção de usar aplicativo, vai aparecer um QR code, você escaneia isso e o Google Authenticator vai passar a te mostrar aquele token. Na hora do login, ele pede o usuário, senha e o token de acesso. 

## Criando a primeira instância EC2

Acesso ao console: https://console.aws.amazon.com/console/

Digira "EC2" na barra de busca:

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a01pt05img1.PNG?raw=true)

Vamos criar a instância EC2 no dashboard. 
Ele é importante para você ter uma fotografia do que está acontecendo.

Note que a região está configurada como Norte da Virgínia. 
Cada região tem um dashboard próprio e uma precificação diferente. 

:warning: Cuidado para não misturar as regiões!

Para criar a instancia clique em "Running Instances".

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a01pt05img2.PNG?raw=true)

Na páginina das Instances, clique em Launch Instance.

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a01pt05img3.PNG?raw=true)

O processo de criação de instancia possui 7 passos:

  1. Choose AMI
  2. Choose Instance Type
  3. Configure Instance 
  4. Add Storage 
  5. Add Tags 
  6. Configure Security Group
  7. Review

![alt text](https://github.com/asalunai/alura-deploy-amazon-ec2/blob/main/a01pt05img4.PNG?raw=true)

Vamos por cada um deles:

### Choose AMI

Ele pede para eu selecionar qual a imagem de máquina eu quero utilizar. 
Ele tem as máquinas Linux, que é o default, e tem máquina Windows também. 

:pushpin: Atente para o label: Free Tier.

Se você tem uma aplicação específica, por exemplo, a minha app é recomendada que rode no Ubuntu. 
Você aciona aqui uma imagem do fabricante, o que foi homologado. 

Se não houver nenhuma especificação, recomenda-se usar o Amazon Linux. 

### Choose Instance Type

Nosso padrão: Amazon Linux EC2 t2.micro. 

:pushpin: Atente para o label: Free Tier.

### Configure Instance 

Por enquanto, deixa o padrão que já está na tela. Não é necessario fazer nenhuma modificação.

### Add storage. 

Você tem uma máquina virtual e tem o disco dessa máquina. São serviços separados, a cobrança disso é separado. 

Deixa o padrão também, está dentro do Free Tier.

:pushpin: Atente para o label: Free Tier.

### Configure Security Group

:warning: *Aqui é muito importante*. 

No Security Group, a máquina está dentro da AWS.
Como é que eu, daqui do escritório, da empresa, de casa, acesso essa máquina? 
Eu tenho que chegar dentro da infraestrutura da AWS.

O Security Group é como se fosse o Firewall que você configura. 
Para que eu possa chegar lá, eu preciso de acesso SSH na porta 22.
Ele vai criar um grupo novo para mim, e eu vou dar o nome aqui de “acesso-remoto”. 

Você clica aqui: My IP. 
Ele colocou o meu IP de saída aqui. No momento, eu estou com esse IP de saída. Só esse IP pode fazer SSH. 
Se o teu IP for dinâmico, isso daqui a pouco vai parar de funcionar e você vai ter que ajustar a regra. 
Fica ligado aqui. Você pode definir ranges de rede, por exemplo, enfim. Vou botar aqui My IP só para nós lançarmos a máquina. 

Review e Launch. Ele dá um resumo do que nós criamos. Lançar máquina.

[09:10] “E daí, Ricardo?” Eu já tenho uma chave criada, mas eu vou criar uma nova. Se você não tiver nenhuma, vai ser forçado a criar. Você precisa definir um nome para essa chave. Eu vou botar “aws-ricardo”. Download. Ponto importante aqui, se você perder essa chave, o que vai acontecer? Você não vai ter mais acesso à sua máquina virtual. Não vai ter acesso. Tem que guardar exatamente essa chave aqui, não pode esquecer disso.

[09:47] Com a chave criada, eu vou lançar a instância. O que ele está fazendo? Está provisionando a instância. Está botando aquela máquina Linux com aquelas configurações no ar. Como é que nós vamos ver isso, como é que nós vamos acessar, como é que eu consigo mexer na máquina? Na sequência, eu te mostro.
