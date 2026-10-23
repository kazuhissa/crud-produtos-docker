CRUD de Produtos com Deploy Automatizado (CI/CD + Docker)Este projeto consiste em uma aplicação web CRUD (Create, Read, Update, Delete) desenvolvida em PHP e MySQL, containerizada com Docker e com fluxo de integração e implantação contínuas (CI/CD) automatizado via GitHub Actions através de um Self-Hosted Runner em uma máquina virtual Ubuntu Server.Arquitetura do AmbienteAplicação Web: PHP 8.2 com ApacheBanco de Dados: MySQL 8.0Orquestração de Containers: Docker e Docker ComposeServidor de Implantação (Host): Ubuntu Server VM (10.11.51.172)Automação CI/CD: GitHub Actions com Self-Hosted Runner (ubuntuserver-trabalho) executado via systemdFluxo de CI/CD (GitHub Actions)

A cada novo git push ou pull request enviado para a branch principal (main), o GitHub Actions aciona automaticamente o runner privado hospedado no Ubuntu Server.O arquivo de pipeline em .github/workflows/deploy.yml executa as seguintes etapas:Checkout do código: Baixa a versão mais recente do repositório (actions/checkout).Atualização da infraestrutura: Recria as imagens Docker e reinicia os serviços em segundo plano:Bashdocker compose down
docker compose up -d --build

Validação do ambiente: Exibe o status dos containers em execução no terminal do workflow (docker ps).Configuração do Self-Hosted Runner no ServidorO runner foi instalado no diretório do usuário na VM e configurado como um serviço nativo do sistema (systemd), garantindo execução contínua em segundo plano e reinício automático caso a máquina virtual seja reiniciada.Sincronização de Horário (NTP)Para prevenir falhas de autenticação de token por inconsistência de relógio (time skew), a máquina virtual conta com sincronização automática de tempo e fuso horário ajustado:Bashsudo apt update && sudo apt install -y systemd-timesyncd
sudo systemctl enable --now systemd-timesyncd
sudo timedatectl set-timezone America/Sao_Paulo
Registro e Instalação do ServiçoComandos executados para registrar o runner ubuntuserver-trabalho e vincular ao repositório:Bashcd ~/actions-runner

Limpeza e configuração do runner
./config.sh --url https://github.com/kazuhissa/crud-produtos-docker --token <TOKEN_DE_REGISTRO>

Instalação e inicialização do serviço systemd
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status

Como Acessar a AplicaçãoCom os containers em execução e o deploy concluído com sucesso, a aplicação fica acessível na rede local através da porta mapeada:Aplicação Web: http://10.11.51.172:8080Status da ImplantaçãoRecursoStatusObservaçãoContainers DockerAtivocrud-produtos-app na porta 8080Runner GitHubActiveNome: ubuntuserver-trabalhoPipeline CI/CDSucessoDeploy automático no git push
