CRUD de Produtos com Deploy Automatizado (CI/CD + Docker)

Nome: Gabriel Kazuhissa Aizawa Lucio - RA: 250447

Visão Geral do Projeto

Este projeto consiste em uma aplicação web CRUD (Create, Read, Update, Delete) em PHP e MySQL, containerizada com Docker e com fluxo de CI/CD automatizado via GitHub Actions através de um Self-Hosted Runner em uma máquina virtual Ubuntu Server (10.11.51.172).

Explicação do Workflow de Deploy (.github/workflows/deploy.yml)

Nome da automação

name: CI/CD Docker PHP
Define o nome da Action no painel do GitHub para identificar facilmente o pipeline.

Gatilho de execução

on:
  push:
    branches: [ "main" ]
Configura o fluxo para rodar automaticamente sempre que houver um envio de código (push) na branch principal (main).

Controle de concorrência

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
Cancela execuções antigas que ainda estejam rodando se um novo push for feito, priorizando sempre a versão mais recente.

Definição do Runner Privado e Jobs

jobs:
  build-and-deploy:
    runs-on: self-hosted
Indica que a tarefa vai rodar no nosso servidor privado (a VM Ubuntu Server) em vez de usar as máquinas da nuvem do GitHub.

Passo 1: Baixar o código

      - name: Checkout repository
        uses: actions/checkout@v4
Clona o repositório e baixa a versão mais recente do código-fonte para dentro da pasta de trabalho no servidor.

Passo 2: Reiniciar os containers Docker

      - name: Restart Docker Compose
        run: |
          docker compose down
          docker compose up -d --build
Para e remove os containers antigos com o docker compose down, e em seguida reconstrói e sobe os containers atualizados em segundo plano com o código novo.

Como Acessar e Status

Aplicação Web: http://10.11.51.172:8080

Status: Containers ativos, Runner configurado via systemd e pipeline funcional a cada git push.
