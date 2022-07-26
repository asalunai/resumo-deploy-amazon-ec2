# Escalabilidade e Alta Disponibilidade

## Diagrama da solução e Load Balance

📌 Inserir figura!!!

Esse retângulo aqui de fora representa a nuvem da AWS. 
Lembrando que isso daqui é uma visão bem macro. 
Importante dizer: os meus usuários chegam na AWS somente na porta 80 e 443. 
Somente essa porta para todo mundo é que vai ser permitida para que ele ingresse no nosso Load Balancer.

O Load Balancer vai receber todo esse tráfego. 
E esse tráfego que chega aqui vai ser distribuído para as instâncias. 
Que instâncias são essas? 
Réplicas daquele web cadastro, daquela imagem que nós criamos. 
E essa instância aqui, nós vamos configurar para que ela faça a escala automática.

Nós começamos com duas instâncias. 
Subiu seu tráfego, ele vai e sobe uma nova instância. 
Subiu mais o tráfego, acrescenta uma nova instância, e por aí vai. 
Em caso de falha, ele substitui a instância. 
E no final, todas as instâncias apontam para o mesmo RDS, ou seja, aquele serviço que nós criamos, que é o nosso database com as informações do cadastro.

Como é que eu gerencio um ambiente? 
Pela porta 22: chega aqui na AWS e você consegue fazer acesso às suas instâncias, caso necessário. 
Ou pelo dashboard.

### Load Balancer

Configurando o Load Balancer:

* No menu à esquerda, vá em "Load Balancer" => "Load Balancers"
* No Dashboard, clique em "Create Load Balancer"
* Escolha a opção "Aplication Load Balancer", que é para HTTP e HTTPS
* Em "Configure Load Balancer", escolha "internet-facing"
* Ainda em "Configure Load Balancer", na parte "Availability Zones", escolha a VPC.
* Ainda em "Configure Load Balancer", na parte "Availability Zones", é recomendado escolher pelo menos 2 zonas, para que haja redundância. Aqui, vamos escolher as zonas "a" e "b". Lembre-se disso, todo o resto deve ser configurado para essas zonas.
* Em "Security Group", é uma boa prática criar um grupo específico para o Load Balancer. Aqui crie um grupo com a porta 80 e HTTP.
* Em "Configuring Routing", na parte "Target Group", defina um nome para o TG e escolha "Target Type: Instances", "Port: 80", "Protocol: HTTP"
* Ainda em Em "Configuring Routing", na parte "Health Check" selecione "Protocol: HTTP". Deixe os demais (incluindo as opções avançadas) como padrão.
* Em "Register Targets", apenas pule essa parte. Vamos definir o Autoescaling a seguir e o LB vai registrar sozinho esse grupo que nós vamos criar.
* Revise tudo e finalize o processo.

📌 O Target Group é para onde o Load Balancer olha. O LB olha as instâncias, mas elas têm que estar agrupadas no que ele chama de target group.

⚠️ Em "Register Targets", preste atenção: 
Se voce quer fazer um escalonamento automático, você não vai aprontar para uma instância específica, mas sim para o serviço que vai escalar as máquinas.
E esse serviço é o Autoescaling que vamos configurar a seguir.

📌 Você pode usar o Load Balancer sem serviço de auto scaling. Você pega as suas instâncias e aponta direto em "Register Targets". 

A seguir, vamos associar o Load Balancer ao grupo do Auto Scaling.

## Configurando o Auto Scaling

### Criando um Auto Scaling Configuration:

* No menu à esquerda, vá em "Auto Scaling" => "Launch Configuration"
* No Dashboard, clique em "Create launch configuration"
* Em "1. Choose AMI", vá em "My AMIs" e selecione a imagem que criamos na aula anterior (web-cadastro)
* Em "2. Choose Image type", mantenha o padrão Free Tier
* Em "3. Configure Details", defina um nome (AS-config-webcadastro) e deixe o resto como padrão
* Em "4. Add Storage", mantenha o padrão
* Em "5. Security Group", selecione os grupos: acesso-web (HTTP+HTTPS), acesso-remoto (SSH) e default (VPC padrão: usar a mesma do LB, ver zonas de disponibilidade)
* Review e Launch!

⚠️ No security group: Lembre-se: o Load balance vai falar com a instancia Auto Scaling na porta 80 (VPC)!

⚠️ No security group: Lembre-se: a instancia Auto Scaling vai falar com o Banco de Dados (RDS) atraves da VPC!

⚠️ No security group: Lembre-se: a instancia Auto Scaling vai receber tráfego atraves do acesso HTTP/HTTPS!

⚠️ No security group: Lembre-se: o gerenciamento da instancia será feita pelo SSH!

### Criando um Auto Scaling Group:

* Volte no menu à esquerda e vá em "Auto Scaling" => "Auto Scaling Groups"
* No dashboard, clique em "Create Auto Scaling Group"
* Escolha a opção "Launch Configuration"
* Selecione a configuração que acabamos de criar (AS-config-webcadastro)
* Em "1. Configure AS group details", defina um nome (AS-group-cadastroWeb)
* Em "1. Configure AS group details", em "Group size" defina 2 (assim começamos com 2 maquinas)
* Em "1. Configure AS group details", em "Network" selecione a VPC padrão (a mesma de todo o resto!)
* Em "1. Configure AS group details", em "Subnet" selecione as zonas "a" e "b" (lembra do "Availability Zones" do LB? são elas mesmas)
* Em "1. Configure AS group details", em "Advance details": selecione "Receive trafic from Load Balancer(s)"
* Em "1. Configure AS group details", em "Advance details", "Target groups": selecione o TG que criamos no LB
* Em "1. Configure AS group details", em "Advance details", "Health Check": selecione a opção "ELB" (Elastic Load Balancer)
* Em "1. Configure AS group details", em "Advance details": deixe os demais como padrão
* Em "2. Configure Scaling Policies": selecione "keep this group at its initial size". Depois isso pode ser alterado conforme a necessidade.
* Em "3. Configure Notifications": por enquanto vamos pular isso. Isso é um tema mais avançado
* Em "4. Configure Tags": pula também
* Review e Create!

📌 Depois de criado, ainda é possivel fazer ajustes no AS Group. Então não se preocupe muito com os detalhes agora. 

⚠️ Em "Subnet”, cuidado: se selecionar as redes erradas, o Load Balance vai falar com a sub-rede que não vai fazer parte do grupo e não vai funcionar.
Se eu criei um grupo que escala as máquinas entre duas subnets A e B, essas máquinas vão estar disponíveis nessas subnets. 
O Load Balance tem que olhar exatamente para essas redes.

⚠️ Em "1. Configure AS group details", em "Advance details". 
É importante selecionar "receive from LB" para que, lá no LB esse AS Group apareça como uma opção. 
Se não ele não vai deixar selecionar.

⚠️ Em "1. Configure AS group details", em "Advance details", "Target groups": 
Lembra que falamos durante a criação do LB, na parte "Register Target" que iríamos pular isso já que o LB iria puxar sozinho do AS? 
É justamente aqui que fazemos essa amarração!

## Testando o ambiente de produção

Verificando as instancias criadas:

* No dashboard do Auto Scaling, vá na aba "Instancias"
* Repare que tem 2 instancias rodando e com o Health Status como "Healthy"
* Repare que tem uma instancia em cada região (ou seja, se uma região ficar indisponivel, ele cria mais uma maquina na outra)
* Se voce voltar no dashboard da EC2, voce vai ver que tem 2 instancias sem nome rodando.
* Voce pode terminar uma das instancias manualmente e ver o que acontece:
    * voltando lá no AS e ver que o status dela estará como "Unhealthy"
    * o sistema irá então derrubar essa máquina e criar outra automaticamente
    * em alguns minutos, voce terá 2 maquinas rodando de novo

Testando a nossa aplicação:

* Vá no dashboard do Load Balance
* Selecione o LB da aplicação
* Copie o DNS name
* Cole esse DNS em uma aba do navegador
* a página de cadastro deverá carregar

⚠️ Preste atenção na quantidade máxima de instancias rodando ao mesmo tempo para não estourar o Free Tier ou o seu cartão de crédito! 

## Domínio e politicas de Auto Scaling

### Domínio

Voce pode pegar aquele DNS name e jogar em um domínio. 
Um exemplo simples é usar o Freenom.

A AWS me disponibiliza o endereço que é do tipo A. 
No Freenom, para cadastrar o DNS do Load Balance, é necessário escolher a opção CNAME.

### Politicas de Auto Scaling

Exemplo: Sua aplicação é uma pagina de vendas e chegou a Black Friday. 
Tem duas máquinas funcionando, começou a vir muito tráfego em cima delas, é legal você disponibilizar mais uma. 

Criando uma politica de AS:

* No Dasboard do AS, selecione a sua AS Group
* Vá na aba "Scaling policies" e clique em "Add Policy"
* No nosso exemplo, vamos definir uma politica baseada em CPU.
* Em "metric type": escolha "Average CPU utilization"
* Em "Target Value", defina 60%
* Criar!
* Volte no Dashboard, selecione a AS Group onde a politica foi definida
* Clique em "Actions" => "Edit" na barra superior
* Edite os valores de Capacity desired/min/max (no exemplo: 2/2/6)
   * Desired: é o padrão. em condicões normais de operação, mantenha esse valor
   * Mínimo: o sistema não vai derrubar instancias para menos do que esse valor
   * Máximo: o sistema não vai criar instancias para mais do que esse valor

📌 A politica de auto scaling tanto sobe quanto derruba uma instancia conforme a necessidade. 
Assim, ela se adapta à dinâmica da operação de maneira que voce tenha uma utilização eficiente de recursos.
