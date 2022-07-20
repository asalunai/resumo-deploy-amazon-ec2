# Imagens e Elastic IP

## Trabalhando com imagens

Criando uma imagem de máquina:

* Pare a instância que será a referencia da imagem (no caso, a web-dev)
* Com a instancia selecionada, vá em "Action" => "Image" => "Create Image"
* Insira um nome para a imagem (web-dev-template) e confira o tamanho do disco associado (no nosso exemplo, 8Gb)
* Depois de criada, ela pode ser vista no menu à esquerda em "Imagens" => "AMIs"
* Quando for criar uma instancia nova, ela estará em "Choose AMI" => "My AMIs"

## IP Dedicado

Atribuindo o IP fixo para as máquinas:
* No menu esquerdo, vá em "Network and Security" => "Elastic IPs".
* Clique em "Allocate Elastic IP Address"
* Seleciona "Amazon's Pool of IPv4 Addresses" => "Allocate"
* Volte em "Network and Security" => "Elastic IPs"
* Selecione o IP criado
* Vá em "Action" => "Associate Elastic IP Address"
* Em "Resource Type", selecione a opção "Instance"
* Em seguida, selecione a instância 
* Clique em Associate

⚠️ Custos: Se a máquina tiver no ar, você pode ter um (1) IP estático associado a ela gratuitamente. Os demais serão cobrados. Se essa máquina estiver parada, esse IP gratuito, apesar de estar associado, ele não está em uso naquele momento. E você passa a ser tarifado em cima dessa situação. Tem o IP alocado, mas ele não está em uso naquele momento.

📌 https://aws.amazon.com/pt/premiumsupport/knowledge-center/elastic-ip-charges/
