ORIENTAÇÕES SOBRE O TZAP
Índice
Informações Importantes
Link do GitHub
Dados do Servidor e da Instalação
Atualizar o Servidor e Reiniciar - Passo 01
Instalador do Whaticket - Passo 02
Processo de Instalação no Terminal SSH - Passo 03
Fix nginx.conf
Fix Baileys, Reinício Programado das Aplicações Nodes
Comandos node / pm2
Particularidades VPSDime - Realize esse procedimento antes da instalação
Habilitar EFI, Personalização e Teste Redis
01. Informações Importantes
Utilizar Ubuntu 20.04

Mínimo requerido: 4 vCPU e 6GB RAM
Recomendado: 4 vCPU e 8GB RAM
Instalação

Somente instale quando os endereços de frontend e backend estiverem pingando para o IP do servidor.
Verifique se está propagado em todas as regiões usando: https://dnschecker.org
Regras de nomeação

Não utilize caracteres especiais, letras maiúsculas ou números no nome do app.
Verifique portas e firewall

As portas utilizadas são: 22, 80, 443, 3000, 4000, 5000, 5432, 6379.
Se o servidor não acessa diretamente com o usuário root, habilite com sudo su ou sudo passwd root.
Usuário e empresas no sistema

Nunca exclua o usuário admin (ID 1) e a empresa criada durante a instalação.
Ao editar o superadmin, não se esqueça de definir uma senha.

Envio de campanhas

Ative o envio de campanhas em "Configurações e Empresas". Após ativar, faça logout e login novamente.
Gerenciamento de horários

Ao alterar o gerenciamento de horários, esvazie os campos preenchidos do formato anterior para evitar conflitos no banco de dados.

02. Link do GitHub
Instalador:
https://github.com/thigranpeio/TZap24_install.git

Código:
https://github.com/thigranpeio/TZap24.git

03. Dados do Servidor e da Instalação
Frontend: [[app.dominio.com]]
Backend/API: [[api.dominio.com]]
App: [[nomedoapp]]
Senha deploy: [[senha-alfanumérica]]
Usuário super-admin: admin@admin.com
Senha: 123456

04. Passo 01 - Atualizar o Servidor
Atualizar pacotes:
sudo apt -y update && apt -y upgrade

Reiniciar o servidor:
reboot

05. Passo 02 - Baixar o Instalador do GitHub
Execute esses comandos no cliente SSH (recomendado Bitvise):
cd /home
ls
sudo apt install -y git && git clone https://github.com/thigranpeio/TZap24_install.git instalador && sudo chmod -R 777 instalador && cd instalador && sudo ./install_primaria

06. Passo 03 - Executar a Instalação
Siga o passo a passo do terminal exibido. Tenha atenção ao copiar e colar para não incluir espaços no final da expressão:

[0] Instalar

Insira a senha para o usuário Deploy e Banco de Dados (não utilize caracteres especiais ou letras maiúsculas):
[[senhadeploy]]

Insira o link do GitHub do seu Whaticket que deseja instalar:
https://github.com/thigranpeio/TZap24.git

Informe um nome para a Instância/Empresa que será instalada (utilize somente letras minúsculas):
tzap

Informe a quantidade de Conexões/Whats que poderá cadastrar (de 1 a 9999):
9999

Informe a quantidade de Usuários/Atendentes que poderá cadastrar (de 1 a 9999):
9999

Digite o domínio do FRONTEND/PAINEL, exemplo:
app.appbot.cloud

Digite o domínio do BACKEND/API, exemplo:
api.appbot.cloud

Digite a porta do FRONTEND, exemplo:
3000

Digite a porta do BACKEND, exemplo:
4000

Digite a porta do REDIS/AGENDAMENTO, exemplo:
5000

07. Fix nginx.conf
Corrigir erros de atualização da página.
Edite o arquivo de configurações do nginx:

nano /etc/nginx/nginx.conf

Adicione o seguinte header (imediatamente acima de # server_tokens off;):
underscores_in_headers on;

Feche e salve as alterações pressionando Ctrl + X, depois Y.

Teste as alterações do nginx:
nginx -t

Reinicie o serviço:
service nginx reload

08. Fix Baileys - Reinício Programado das Aplicações Node
Utilize as tarefas agendadas (cron jobs) para programar o comando pm2 restart all:
crontab -e

Se você tiver vários editores de texto instalados, o sistema solicitará que você selecione um editor. Use o número correspondente para escolher o editor, como nano (opção padrão).

Exemplo de cron job que reinicia o frontend e o backend todo dia às 23h59:
59 23 * * * /usr/bin/node /usr/bin/pm2 restart all

Para criar outros modelos de cron, consulte um dos sites abaixo:
https://crontab.guru/
https://crontab-generator.org/
https://crontab.cronhub.io/

09. Comandos node / pm2
Comando para build/rebuild:
sudo npm run build

Comando para atualizar as bibliotecas:
npm update --force

Comando para reiniciar pm2:
pm2 restart all

Logs do pm2:
pm2 log

Limpeza de logs do pm2:
pm2 flush "id"

Alterar Nome, Cor Primária, Logotipo e Favicon
Alterar cor primária (#2DDD7F):
/frontend/src/App.js
/frontend/src/layout/index.js

Alterar cores do Chat Interno:
/frontend/src/pages/Chat/ChatMessages.js
/frontend/src/pages/Chat/ChatList.js

Alterar cores da Lista de Tarefas:
/frontend/src/pages/ToDoList/index.js

Alterar logo e logotipo de login:
/frontend/src/assets

Alterar ícone e favicon:
/frontend/public

Comando para rebuild (caminho absoluto /home/deploy/"nome"/):
cd /frontend
npm run build

Acesso ao PostgreSQL
Utilize o PGAdmin4 com as credenciais do banco de dados salvas no arquivo .env do backend, além dos dados de acesso root, IP e senha para configurar o túnel.
Redis - Banco de Dados de Agendamento
Sequência de comandos para testar se a senha do Redis funciona:

Testar com Docker:
docker ps
docker exec -it CONTAINER_ID bash
redis-cli
auth sua_senha

Testar sem Docker:
redis-cli
set key1 10

Caso não funcione, você verá o erro:
(error) NOAUTH Authentication required.

Autenticar com a senha configurada no Redis:
auth sua_senha

Testar conexão com Redis:
ping
Resposta esperada: pong

Sair do cliente Redis:
quit

ATUALIZAR O TZAP

1-FRONTEND

npm install



caminho da pasta do frontend cd /home/deploy/tzap/backend

2-BACKEND

npm install
npm run build
npx sequelize db:migrate
npm run build

3-FRONTEND

npm run build


aperte cd para apagar o caminho

4-pm2 restart all
pm2 save
