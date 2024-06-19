sudo apt -y update && apt -y upgrade

sudo apt-get install ufw
sudo ufw enable
sudo ufw allow 22/tcp && sudo ufw allow 80/tcp && sudo ufw allow 443/tcp && sudo ufw allow 3000/tcp && sudo ufw allow 4000/tcp && sudo ufw allow 5000/tcp && sudo ufw allow 5432/tcp && sudo ufw allow 6379/tcp
sudo ufw status

sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -

reboot
====================================================================
====================================================================

cd /home
ls
sudo apt install -y git && git clone https://github.com/launcherbr/instalador.git instalador && sudo chmod -R 777 instalador  && cd instalador  && sudo ./install_primaria

- Insira senha para o usuário Deploy e Banco de Dados (Não utilizar caracteres especiais ou letras maiúsculas)
[[senhadeploy]]

- Insira o link do GITHUB do seu Whaticket que deseja instalar:
[[link]]

- Informe um nome para a Instancia/Empresa que será instalada (Não utilizar espaços, caracteres especiais ou letras maiúsculas, utilize somente letras minúsculas;):
[[nomedoapp]]

- Informe a Qt. de de Conexões/Whats que a poderá cadastrar: 1 a 9999
9999

- Informe a Qt. de Usuários/Atendentes que a poderá cadastrar: de 1 a 9999
9999

- Digite o domínio do FRONTEND/PAINEL; Ex: app.appbot.cloud
[[app.dominio.com]]

- Digite o domínio do BACKEND/API; Ex: api.appbot.cloud
[[api.dominio.com]]

- Digite a porta do FRONTEND para a appbot; Ex: 3000 A 3999
3000

- Digite a porta do BACKEND para esta instancia; Ex: 4000 A 4999
4000

- Digite a porta do REDIS/AGENDAMENTO MSG para a appbot; Ex: 5000 A 5999 = padrão dos redis é 6379 / na instalação com docker usar 5000
5000

e aguarde o fim da instalação!
====================================================================
====================================================================
