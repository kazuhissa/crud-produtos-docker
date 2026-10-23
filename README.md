CRUD de Produtos com Deploy Automatizado (CI/CD + Docker)
Este projeto consiste em uma aplicação CRUD em PHP e MySQL rodando em containers Docker, com um fluxo de deploy automatizado via GitHub Actions utilizando um Self-Hosted Runner em uma máquina virtual Ubuntu Server.

Arquitetura do Ambiente
Aplicação: PHP com Apache

Banco de Dados: MySQL 8.0

Orquestração: Docker Compose

Servidor de Deploy: Ubuntu Server 24.04 (VirtualBox - IP 192.168.56.102)

Automação: GitHub Actions (Runner local configurado como serviço systemd)

Fluxo de CI/CD
Sempre que alterações são enviadas para a branch main (ou master), o GitHub Actions aciona o runner hospedado na máquina virtual para executar a atualização da aplicação.

O arquivo de workflow (.github/workflows/deploy.yml) executa os seguintes passos:

Baixa a versão mais recente do código (actions/checkout).

Recria e reinicia os containers da aplicação (docker compose down e docker compose up -d --build).

Exibe o status dos containers ativos (docker ps).

Configuração do Runner no Servidor
O runner do GitHub foi instalado fora do diretório da aplicação e configurado para rodar em segundo plano como um serviço do sistema Ubuntu (systemd).

Comandos utilizados no servidor para registro e execução contínua:

Bash
cd ~/actions-runner
./config.sh --url https://github.com/kazuhissa/crud-produtos-docker --token <TOKEN>
sudo ./svc.sh install
sudo ./svc.sh start
Como Acessar
Com os containers em execução, a aplicação fica disponível na rede local através da interface Host-Only:

URL: [http://192.168.56.102:8080](http://192.168.56.102:8080)
