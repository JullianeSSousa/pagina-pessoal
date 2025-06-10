
Link/Ip da instância para acessar a página: http://18.231.36.185/



Passo a Passo para a criação do projeto


1. Criar uma Instância EC2 com Ubuntu
Passo a passo:
Acesse o painel AWS EC2:
 https://console.aws.amazon.com/ec2


Clique em “Launch Instance”


Preencha:


Nome da instância: meu-site


AMI: Ubuntu Server 22.04 LTS


Tipo de instância: t2.micro (grátis no Free Tier)


Par de chaves: crie ou selecione uma .pem
 (guarde com segurança, você vai precisar para acessar via SSH)

Como adicionar regras no Security Group (durante a criação)
Na etapa “Configure Security Group”:
Clique em “Create new security group”
 (não use um grupo antigo, crie um novo para facilitar)


Preencha o nome e a descrição (ex: web-access-group)


Em “Inbound rules” (Regras de entrada), clique em “Add Rule” e adicione:
Tipo
Protocolo
Porta
Origen
Descrição
SSH
TCP
22
My IP
Acesso via terminal SSH
HTTP
TCP
80
0.0.0.0/0
Acesso à sua página web
Custom TCP
TCP
8080
0.0.0.0/0
Se usar http.server



⚠️ Importante:
Use "My IP" para a porta 22 (SSH), pois é mais seguro.


Use "0.0.0.0/0" para portas 80 ou 8080, permitindo que qualquer um acesse seu site via navegador.


Clique em “Launch instance”



2. Encontrar o IP Público da Instância EC2
Vá para EC2 > Instances


Clique na instância que você criou


Na aba “Details”, procure:


IPv4 Public IP ou Endereço IPv4 público


Esse IP será usado para:
Acesso via terminal (SSH)


Acesso à página no navegador (http://SEU_IP) 
(Provavelmente não irá funcionar)

3. Conectar à EC2 via SSH
No MacOS/Linux (Terminal):
bash
chmod 400 minhachave.pem
ssh -i minhachave.pem ubuntu@SEU_IP

No Windows (PowerShell com OpenSSH):
powershell
icacls.exe .\chave-meu-portfolio.pem /reset
icacls.exe .\chave-meu-portfolio.pem /grant:r "$($env:username):(r)"
icacls.exe .\chave-meu-portfolio.pem /inheritance:r

4. Configurar o Servidor Web
Depois de conectar via SSH à sua instância:
Instale pacotes:
bash
sudo apt update
sudo apt install nginx python3 -y

Inicie o nginx:
bash
sudo systemctl start nginx

Teste no navegador:
http://SEU_IP 
(Após instalar e iniciar o nginx com os comandos acima, essa URL deve funcionar)

Você verá a página padrão do nginx.

5. Criar sua página Web localmente
No seu computador, crie seu projeto na seguinte estrutura:
meu-site/
├── index.html
├── style.css
└── perfil.jpg




6. Enviar os arquivos para a instância EC2
No MacOS (Terminal):
bash
scp -i minhachave.pem -r ./meu-site/ ubuntu@SEU_IP:/home/ubuntu/

No Windows (PowerShell com OpenSSH):
powershell
scp -i C:\caminho\da\minhachave.pem -r C:\caminho\para\meu-site\ ubuntu@SEU_IP:/home/ubuntu/


7. Servir os arquivos na web
Usando o nginx:
bash
sudo cp -r /home/ubuntu/meu-site/* /var/www/html/

Acesse no navegador:
cpp
http://SEU_IP/


