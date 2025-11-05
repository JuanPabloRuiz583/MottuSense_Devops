# Proposta

MottuSense é uma solução inteligente desenvolvida para a Mottu, focada no mapeamento automatizado do pátio e na gestão eficiente das motos da frota.
Utilizando sensores IoT e uma arquitetura em nuvem com APIs .o sistema permite monitoramento em tempo real, controle de entrada e saída dos veículos, e integração com um app mobile para operadores.
Combinando banco de dados relacional e não relacional, DevOps, testes de qualidade e tecnologias modernas de desenvolvimento mobile e backend, o MottuSense garante rastreabilidade, performance e escalabilidade — tudo alinhado com os pilares da Mottu: tecnologia, mobilidade e oportunidade.

🛵 Nome da Solução: MottuSense
🔤 Significado:
"Mottu" (nome da empresa)
"Sense" de sensorial, percepção, inteligência → representa a capacidade da solução de "sentir" e gerenciar o pátio de motos com IoT.

## Diferencial

- Monitoramento em tempo real das motos atraves dos sensores
- localização exata das motos

## Beneficios para o negocio

- rapido gerenciamento de motos que estao inativas e em uso,pelo nosso site
- busca de motos por placa
- sensores que disponibilizam a localização exata da moto atraves da latitude e longitude

# Mottu Sense

Aplicação web MVC para gestão de motos, sensores de localização e pátios, com autenticação de usuários.possui crud completo(create,update,delete e listar) para motos e sensores

## Tecnologias

- Java
- Spring Boot
- Gradle
- Thymeleaf
- Flyway
- Postgres
- Spring Security
- oAuth2 (permitindo se autenticar com o github ou google)
- Seeder (deixando patios ja cadastrados automaticamente no sistema)
- compose.yaml 





# Instalação do projeto e instruções passo a passo para criar banco de dados + acr + appservice, via cli (obs: para o projeto funcionar sem erros os seguintes comandos abaixo precissam ser seguidos)

1. **Clone o repositório:**
   ```bash
   https://github.com/JuanPabloRuiz583/MottuSense_Devops.git

2. **Criar grupo de recurso :**
   ```bash
   az group create --name mottusense-rg --location eastus


3. **Criar SQL Server:**
   ```bash
   az sql server create --name mottusense-sqlsrv-br --resource-group mottusense-rg --location brazilsouth --admin-user admin_fiap --admin-password Teste123!


4. **Criar banco de dados no SQL Server:**
   ```bash
   az sql db create --resource-group mottusense-rg --server mottusense-sqlsrv-br --name mottusensedb --service-objective S0


5. **Criar regra de firewall para liberar acesso:**
   ```bash
   az sql server firewall-rule create --resource-group mottusense-rg --server mottusense-sqlsrv-br --name AllowAllIPs --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255



6. **copie os scripts de criação de tabelas do meu arquivo script_db.sql, entre depois no query editor do portal azure e cole os scripts la e execute**



7. **Criar acr com a opcao de admin user:**
   ```bash
   az acr create --resource-group mottusense-rg --name rm557727 --sku Standard --location "East US" --admin-enabled true


   

8. **Criar App Service Plan:**
   ```bash
   az appservice plan create --name mottusense-plan --resource-group mottusense-rg --sku B1 --is-linux --location eastus




9. **Criar App Service**
   ```bash
   az webapp create --resource-group mottusense-rg --plan mottusense-plan --name mottusense-app --deployment-container-image-name nginx



10. **Configurar variáveis de ambiente no App Service:**
    ```bash
    az webapp config appsettings set --resource-group mottusense-rg --name mottusense-app --settings WEBSITES_PORT=8080 GITHUB_CLIENT_ID=Ov23liPExW7Z4g4CtLOY GITHUB_CLIENT_SECRET=3d334f3113c1890485ccc6fa39c27102bf512b84 GOOGLE_CLIENT_ID=412634895320-k0f2uesevgp6k3dulemambo97rd3qn2o.apps.googleusercontent.com GOOGLE_CLIENT_SECRET=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 SPRING_DATASOURCE_URL="jdbc:sqlserver://mottusense-sqlsrv-br.database.windows.net:1433;database=mottusensedb;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30" SPRING_DATASOURCE_USERNAME=admin_fiap SPRING_DATASOURCE_PASSWORD="Teste123!" DOCKER_REGISTRY_SERVER_URL=rm557727.azurecr.io DOCKER_REGISTRY_SERVER_USERNAME=rm557727 DOCKER_REGISTRY_SERVER_PASSWORD=XE0Pq+UTMP2PFFvrDcIJFrj1zXl2VvGeaZS92LTE/h+ACRAaoog3




11. **escolher a imagem docker do meu acr que sera usada no web app (verifique no acr em repositories qual é a versao da imagem):**
    ```bash
    az webapp config container set --name mottusense-app --resource-group mottusense-rg --docker-custom-image-name rm557727.azurecr.io/sprint4:20251105.11 --docker-registry-server-url rm557727.azurecr.io --docker-registry-server-user rm557727 --docker-registry-server-password "XE0Pq+UTMP2PFFvrDcIJFrj1zXl2VvGeaZS92LTE/h+ACRAaoog3"




12. **Se tudo foi seguido corretamente a aplicação estara disponivel, Acesse no navegador:**

🔑 Login (autentique-se primeiro):

http://mottusense-app.azurewebsites.net/login

🏍️ Motos — Cadastro / Edição / Remoção / Listagem / Busca por placa:

http://mottusense-app.azurewebsites.net/moto
(se não estiver autenticado, será redirecionado para a tela de login)

📄 Formulário de Motos:

http://mottusense-app.azurewebsites.net/moto/form
(acessível também clicando no botão "Nova moto")

🏢 Pátios — Listagem (ver quais pátios estão disponíveis antes do cadastro):

http://mottusense-app.azurewebsites.net/patio

📍 Sensores — Cadastro / Edição / Remoção / Listagem:

http://mottusense-app.azurewebsites.net/sensor-localizacao
(para criar, é necessário ter uma moto cadastrada para vincular a placa)

📝 Formulário de Sensores:

http://mottusense-app.azurewebsites.net/sensor-localizacao/form
(acessível também clicando no botão "Cadastrar sensor")

🔒 Logout:

http://mottusense-app.azurewebsites.net/logout



