ORIENTAÇÕES SOBRE O TZAP

Índice:

01. Informações Importantes
02. Link do GitHub
03. Dados do Servidor e da Instalação
04. Atualizar o Servidor e Reiniciar - Passo 01
05. Instalador do Whaticket - Passo 02
06. Processo de instalação no terminal ssh - Passo 03
07. Fix nginx.conf
08. Fix Baileys, reinício programado das aplicações nodes
09. Comandos node / pm2
10. Particularidades VPSDime - Procedimento antes da instalação
11. Habilitar Efi, Personalização e Teste Redis
12. Canais de Ajuda
13. Instalação com Docker / Ubuntu 22
14. Como Atualizar o TZAP

=====================================================================================

01. Informações Importantes

- Utilize Ubuntu 20.04
- Mínimo requerido: 4 vcpu e 6gb RAM
- Recomendado: 4 vcpu e 8gb RAM

** Somente instale quando os endereços de frontend e backend estiverem pingando para o IP do servidor.

- Confira em https://dnschecker.org/ se está propagado em todas as regiões.
- Não utilize caracteres especiais, letras maiúsculas ou números no nome do app.
- Confira se o provedor de serviço VPS tem firewall e libere as portas 22, 80, 443, 3000, 4000, 5000, 5432 e 6379.
- Caso o servidor não acesse direto com root, use `sudo su` ou `sudo passwd root` para habilitar o login root.

** Nunca delete o usuário admin (id 1) ou a empresa criada na instalação.
** Habilite o envio de campanhas em "configurações" e "empresas". Após ativar, faça logout e login para o menu aparecer.
** Exclua o Plano 1 e vincule outro plano à empresa 1 antes de adicionar novas conexões, filas e usuários.
** Ao mudar gerenciamento de horários, limpe os campos de horário anteriores.

=====================================================================================

02. Link do GitHub

- Instalador: https://github.com/thigranpeio/TZap24_install.git
- Código: https://github.com/thigranpeio/TZap24.git

=====================================================================================

03. Dados da Instalação

- Frontend: [[app.dominio.com]]
- Backend/API: [[api.dominio.com]]
- App: [[nomedoapp]]
- Senha Deploy: [[senha-alfanumérica]]
- Usuário super-admin: admin@admin.com
- Senha: 123456

=====================================================================================

04. Passo 01 - Atualizar o Servidor

Atualize os pacotes do Ubuntu com privilégios root:

sudo apt -y update && apt -y upgrade


Reinicie o servidor:

reboot


=====================================================================================

05. Passo 02 - Baixar o Instalador do GitHub

Execute os comandos no terminal SSH:

cd /home sudo apt install -y git && git clone https://github.com/thigranpeio/TZap24_install.git instalador && sudo chmod -R 777 instalador && cd instalador && sudo ./install_primaria



=====================================================================================

06. Passo 03 - Executar a Instalação

Siga o passo a passo do terminal.

Exemplo de preenchimento:

- Conexões/Whats: 9999
- Usuários/Atendentes: 9999
- Domínio Frontend: [[app.dominio.com]]
- Domínio Backend: [[api.dominio.com]]
- Porta Frontend: 3000
- Porta Backend: 4000
- Porta Redis: 5000

=====================================================================================

07. Fix nginx.conf

Edite o arquivo de configurações do nginx: 
nano /etc/nginx/nginx.conf

adicione o header abaixo: (imediatamente acima de # server_tokens off;)

underscores_in_headers on;

+ Ctrl e X para fechar o terminal, e Y para salvar as alterações.

Teste as alterações do nginx:

nginx -t

Reinicie o serviço:

service nginx reload


=====================================================================================

08. Fix Baileys e Reinício Programado

Agende o reinício diário das aplicações às 23h59 no cron:

crontab -e

59 23 * * * /usr/bin/node /usr/bin/pm2 restart all

=====================================================================================

09. Comandos Node / PM2
Comando para build/re-build:

sudo npm run build

Comando para atualizar as bibliotecas:

npm update --force

Comando para reiniciar PM2:

pm2 restart all

Ver logs:

pm2 log

Limpeza de logs:

pm2 flush "id"

10. Procedimentos para VPSDime
Atualizar pacotes do servidor:

sudo apt -y update && apt -y upgrade

Instalar e habilitar UFW (firewall):

sudo apt-get install ufw
sudo ufw enable

Liberar portas:

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 3000/tcp
sudo ufw allow 4000/tcp
sudo ufw allow 5000/tcp
sudo ufw allow 5432/tcp
sudo ufw allow 6379/tcp

Verificar portas liberadas:

sudo ufw status

Instalar Curl:

sudo apt-get install curl

Definir versão do Node.js:

curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -

11. Configurações e Teste Redis
Configurações Gerencianet:

Adicionar as seguintes linhas no arquivo .env do backend:

GERENCIANET_SANDBOX=false
GERENCIANET_CLIENT_ID=Client_Id_Gerencianet
GERENCIANET_CLIENT_SECRET=Client_Secret_Gerencianet
GERENCIANET_PIX_CERT=certificado-Gerencianet
GERENCIANET_PIX_KEY=chave_pix_gerencianet

Alterar Cor Primária e Logotipos:

Cor Primária:

frontend/src/App.js
frontend/src/layout/index.js

Logo e Favicon:

frontend/src/assets (logo)
frontend/public (favicon)

Comando para rebuild do frontend:

cd /frontend
npm run build

12. Redis - Teste
Com Docker:

docker ps
docker exec -it CONTAINER_ID bash
redis-cli
auth "sua_senha"

Sem Docker:

redis-cli
auth "sua_senha"

Como Atualizar o TZAP
1. FRONTEND
npm install

Entre na pasta do frontend:
cd /home/deploy/suaempresa/frontend

2. BACKEND
npm install
npm run build
npx sequelize db

npm run build

3. FRONTEND
npm run build

4. Reiniciar PM2
pm2 restart all
pm2 save
