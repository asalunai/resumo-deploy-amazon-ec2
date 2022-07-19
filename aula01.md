# Aula 01 - Primeiros passos na AWS

## Criando uma conta

Link AWS Amazon: aws.amazon.com

Procurar o botão de Criar conta e preenchero formulário.

:pushpin: Nota: É obrigatório cadastrar um cartão de crédito internacional.

:pushpin: Nota: O processo de ativação de conta pode demorar um pouco.  

## Uso gratuito e recomendações inicias

Sobre o Free Tier: https://aws.amazon.com/pt/free/

Sobre preços: https://aws.amazon.com/pt/pricing/

Sobre caluladora de serviços: https://calculator.aws/#/

* A AWS tem alguns serviçoes gratuitos. Eles são divididos em 3 tipos:
  1. 12 meses gratuitos: Quando você cria a sua conta, você ganha 12 meses de uso gratuito. Quando o período de vigência de 12 meses de uso gratuito expirar, ou se a utilização da aplicação ultrapassar os limites do nível gratuito, você pagará taxas de serviço padrão conforme o uso.
    * Instâncias EC2:  t2 micro Linux, ligada por até 750 horas ( ~1 mês inteiro)
    * Armazenamento S3: RDS
  3. Sempre Gratuito: É para sempre. Mesmo passados os 12 meses, como o nome diz, sempre vai ser gratuito. 
  4. Testes: Por exemplo: saiu um serviço novo. Ele te dá um período de teste gratuito naquele serviço. Serviço XPTO, você tem 200 horas para usar. Passou disso, você vai ser cobrado as horas adicionais.

:warning: ATENÇÃO: Sempre leia o regulamento e as letras miúdas para depois não levar susto no cartão de credito!

O Lambda, ele é um FaaS. O FaaS, essas letrinhas bonitas, é aquele Function as a Service. Eu coloco a minha função lá, e não importa o hardware que ele vai rodar. Eu sou cobrado por tempo e por quantidade de memória que eu uso.
O legal é que, dependendo da configuração da minha função, até um milhão de vezes é sempre gratuito. Se eu chamar minha função um milhão de vezes, ocupando 128Mb de memória, e durando a cada segundo, eu consigo chamar um milhão de vezes. Hoje, no meu dia a dia, que a minha conta já passou do período gratuito há muitos anos, eu uso o lambda e não sou cobrado por isso, porque dentro desse perfil, ele é o sempre gratuito.

Machine Learning, Teste Gratuito. Você tem 250 horas desse serviço. Esse material que é digitar AWS, uso gratuito, é fundamental para você ingressar aqui na AWS. E quando foi estudar outro provedor, pegar o equivalente.

Como é que nós acessamos o console? como é que nós usamos o dashboard da AWS? Você acabou de criar a sua conta, independente de onde você estiver, pode botar “AWS console”.

Recomendação de seguranção: É opcional, mas fortemente recomendado. Assim que criar a conta: na barra de busca, digita “iam”. E você ativa o segundo fator de autenticação: “Activate MFA on root account”. Recomendado: Token via o Google Authenticator.
Como é que você vincula o Google Authenticator à sua conta na AWS? Você vem em gerenciar MFA, marca a opção de usar aplicativo, vai aparecer um QR code, você escaneia isso e o Google Authenticator vai passar a te mostrar aquele token. Na hora do login, ele pede o usuário, senha e o token de acesso. 
