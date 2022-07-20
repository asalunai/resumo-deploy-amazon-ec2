# Automatizando a Instância EC2

## Criando uma Instância automatizada

[00:00] De volta aqui ao dashboard, sempre olhando para cá para você ter um resumo do que está acontecendo. Não tem nenhuma instância rodando, mas eu tenho dois volumes. Como assim? São aqueles dois discos referentes às duas instâncias. Se você vier aqui, já sabe, visualiza tudo. Qualquer dúvida, volta sempre no dashboard. Security Group, temos dois. Chaves, eu tenho dois, porque eu já tinha uma minha, criei uma nova. Você, possivelmente, só tem uma chave aqui. Mas até aqui tranquilo, nós já vimos.

[00:35] Você pergunta assim: “Será que dá para eu lançar uma máquina EC2 já pré-customizada, ou seja, um template. Como é que eu faço isso?” Quando nós vamos aqui em lançar instância. Além daqueles modelos que eu te mostrei no início, existem já aqui, AWS marketplace, já existem algumas imagens pré-definidas. Então, vou pegar como exemplo aqui a primeira. Backup & Recovery.

[01:07] Já tem imagens prontas. Você clicou aqui, escolheu esse Acronis Backup Gateway, por exemplo. Estou lendo aqui o da sequência. Você clicou aqui e ele já lança uma instância, já configurada dessa maneira. Por exemplo, quer ver uma coisa que é familiar? “Wordpress”. Será que tem imagem já customizadas para o Wordpress? Botei aqui para procurar, e ele já retornou para mim uma imagem do Wordpress. Vamos dar uma lida aqui só para algum detalhe importante.

[01:43] Essa máquina já vem PHP, vem Apache, o SQL. Tem tudo prontinho lá. É só selecionar aqui. É importante você observar o seguinte: se eu quisesse lançar uma imagem dessas, já customizada, será que o software que eu estou lançando tem algum custo? É importante você sempre olhar essa coluna daqui. Esses valores de EC2 nada mais são do que os valores do preço de tabela AWS. Isso aqui é a computação, o recurso AWS, isso aqui o software se tivesse algum custo adicional.

[02:23] Aqui é uma opção de nós pegarmos uma imagem, um template, e lançar já um Wordpress. Depois nós viríamos na documentação aqui de quem fez a imagem, via como fazer os ajustes. Mas e se eu quiser fazer a minha própria imagem, como é que eu faço isso? Eu poderia vir aqui, simplesmente lançar uma instância, uma Linux, e fazer a instalação toda na mão. A partir daí eu criaria uma imagem. Só que não é uma coisa muito prática para fazer. A ideia é nós pegarmos a partir daqui e já lançarmos direto o nosso template.

[03:03] Como assim? Deixa eu te mostrar a forma mais fácil de nós resolvermos esse problema. Selecionei minha instância. Detalhes. Agora que nós vamos ter atenção aqui para mudar alguns pontos. Número de instâncias que eu quero lançar, posso lançar cinco instâncias ao mesmo tempo. Isso é um detalhe legal. VPC, a rede Subnet. Ele vai associar a rede automaticamente. Vamos descer aqui.

[03:33] Enable Termination Protection, vamos habilitar. É aquela opção que eu te mostrei anteriormente. Isso aqui é legal amarrar. Mas o ponto em si está aqui: User data. Esse campo aqui permite que eu coloque, linha a linha, o script que eu quero fazer. Assim, ele já lança a máquina com ela prontinha do jeito que eu quero. Em vez de nós editarmos aqui, o que eu faço? Eu vou abrir aqui o meu editor.

[04:05] Estou lá no meu diretório labs. Eu vou criar aqui um arquivo “script.sh”. O que eu faço aqui? Eu vou colocando linha a linha do que eu quero fazer. Qual o objetivo? Eu preciso instalar o Apache, eu preciso instalar o PHP e preciso instalar um banco de dados. A configuração para o meu desenvolvedor que ele pediu para eu ver um template, uma máquina assim para ele.

[04:31] Aqui nós escreveríamos o seguinte: “/bin/bash”. Isso aqui é bem a cara de devops, é bem a cara dessas ferramentas que tem aí de Ansible para nós. O Terraform é uma visão um pouquinho diferente. A ideia que é provisionar já um ambiente preparado para trabalhar. Dá uma olhada também na plataforma, porque tem uns curso interessantes sobre esse tema. Mas aqui nós vamos fazer na mão, “/bin/bash”.

[05:03] O que eu quero fazer? A máquina subiu. Primeira providência é atualizar o conteúdo. Nós colocamos lá. Não preciso colocar “sudo”, porque, na máquina, esse script vai rodar como root, eu não preciso colocar. Fica assim: “yum update”. O que você tem que tomar cuidado é: se eu colocar só o “yum update”, ele vai fazer o update, mas o prompt pede uma confirmação, sim ou não. Você já tem que passar todos os parâmetros. Yes aqui.

[05:45] É uma forma como você criar uma imagem do Docker, a ideia é sempre a mesma. Fiz o update. O que mais eu preciso? No meu caso, eu vou adicionar alguns repositórios. A máquina aponta para os repositórios default, que são repositórios da AWS. Eu vou atualizar aqui. Eu vou adicionar esses repositórios extras aqui. Que mais? Vamos continuar. Agora, eu preciso instalar o Apache e o banco de dados.

[06:19] Deixa eu colar a linha aqui para facilitar, “yum install –y”, ele vai instalar o httpd, que é o Apache e o mariadb que é o MySQL. É só nós seguirmos a lógica. Update, coloquei os repositórios com os pacotes atuais. Instalei o meu Apache, o banco de dados.

[06:43] Com ele instalado, isso daqui eu inicializo o serviço do httpd, o Apache. E além de inicializar o serviço, eu tenho que dizer para que toda vez que a máquina seja reiniciada, ele suba automaticamente. Estou dando o comando aqui, o systemctl enable. Assim, ele bota o serviço para carregar automaticamente.

[07:09] E assim como nós fizemos com o Apache, faz com o banco de dados também. Logicamente, eu vou deixar esse script como exemplo, isso só se aplica a essa instalação que eu estou fazendo, para que você possa usar como referência. Mas a ideia é estruturar aqui. Tudo instalado, tudo iniciado. Eu preciso agora fazer o acerto de permissão. O Apache, quando nós instalamos, tem aquela questão do usuário no diretório onde ele vai gravar, tem que colocar ele no grupo. Essas linhas estão aqui.

[07:44] O que elas fazem? “usermod –a –G”. Toma cuidado com isso. Eu pego usuário ec2 user, que é o usuário que nós estamos utilizando, e coloco no grupo Apache. Para quê? Para que eu possa, com esse usuário, de repente criar uma página, criar um conteúdo, alguma coisa do tipo. Coloquei esse usuário no grupo. E aqui, por fim, eu, depois que fiz isso aqui tudo, dei um “chown –R” para ajustar a permissão de tudo que estiver dentro do /var/www vai passar para esse usuário e esse grupo daqui.

[08:24] Por enquanto é só. A ideia aqui não é a aplicação, é te mostrar a infraestrutura. Eu só quero pontuar aqui como é que se resolve esse problema. Está aqui. Vou comentar isso tudo direitinho e deixo para você também no material de apoio. Copiei isso tudo aqui. Copiei. Nós voltamos lá no nosso user data. “Command + V”, e está lá o roteirinho todo que nós fizemos.

[08:55] E o processo aqui é o mesmo. Avançar para adicionar um storage. Essa máquina aqui vai ter 8 GB. Não tem problema. Tag alguma, não. Security group, vamos usar lá o Security Group de acesso, para nós podermos gerenciar a máquina. Review Launch. Launch. Qual chave que vai utilizar? A mesma do início do curso. Está preparando a instância. Na sequência, eu volto para nós vermos se tudo funcionou direitinho.
