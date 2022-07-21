# Insfraestrutura para alta disponibilidade

## Preparando para o Auto Scaling

Precisamos agora, a partir desse nosso último template, gerar uma nova imagem. 
A minha instância, a partir de agora, não precisa mais ter uma base local, já que a ideia é separar.

* Acesse a máquina web-dev
* Execute o comando “sudo systemctl stop mariadb”. Ele vai parar o banco e evitar que inicialize automaticamente.
