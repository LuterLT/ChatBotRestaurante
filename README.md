# 🤖 Chatbot Telegram Restaurante — N8N + Docker

> Automação de atendimento via Telegram utilizando N8N, Docker e Ngrok.

---

## ⚠️ REGRA FUNDAMENTAL DO GRUPO — LEIA ANTES DE QUALQUER COISA

> **Apenas UMA pessoa pode editar o projeto por vez.**

Antes de começar qualquer edição, avise no grupo do WhatsApp que você está editando. Quando terminar, avise novamente que finalizou e que o projeto está livre.

**Isso evita conflitos, perda de trabalho e dor de cabeça.**

---

## ✅ Boa prática obrigatória antes de editar

Antes de abrir o projeto para editar, **sempre** execute os comandos abaixo no terminal dentro da pasta do projeto:

```bash
git fetch
git status
```

Se aparecer `Your branch is up to date`, você está sincronizado e pode editar.
Se aparecer que há atualizações remotas, execute:

```bash
git pull origin main
```

**Nunca edite sem antes verificar se está atualizado.**

---

## 📌 Índice

- [Informações Gerais](#-objetivo-do-projeto)
- [Pré-Requisitos](#-pré-requisitos)
- [Clonagem do Projeto](#-clonagem-do-projeto)
- [Configuração do Git Remote](#-configuração-do-git-remote)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Criação do .env](#-criação-do-env)
- [Criação do docker-compose.yml](#-criação-do-docker-composeyml)
- [Iniciando o Projeto](#-iniciando-o-projeto)
- [Acessando o N8N](#-acessando-o-n8n)
- [Chave de Licença Gratuita do N8N](#-chave-de-licença-gratuita-do-n8n)
- [Configuração do Ngrok](#-configuração-do-ngrok)
- [Configurando o WEBHOOK_URL](#-configurando-o-webhook_url)
- [Importando o Workflow](#-importando-o-workflow)
- [Salvando e Exportando o Workflow](#-salvando-e-exportando-o-workflow)
- [Comandos Úteis](#-comandos-úteis)
- [Git — Enviando Alterações](#-git--enviando-alterações)
- [Persistência de Dados](#-persistência-de-dados)
- [Observações Importantes](#-observações-importantes)

---

## 🎯 Informações Gerais

Este projeto configura um ambiente local de automação utilizando as seguintes tecnologias:

- **N8N** — plataforma de automação de workflows no-code/low-code
- **Docker + Docker Compose** — para rodar o N8N em container isolado
- **Ngrok** — para expor os webhooks do N8N à internet
- **Variáveis de ambiente (.env)** — para configuração segura e centralizada

O N8N será executado dentro de um container Docker. O Ngrok criará um túnel público para que o Telegram (e outros serviços externos) consiga alcançar os webhooks. Os dados do N8N ficarão persistidos localmente na pasta `n8n_data/`.

---

## 🖥️ Pré-Requisitos

Instale as ferramentas abaixo antes de prosseguir:

### Git
Instale o Git na sua máquina. *(Não há instruções detalhadas aqui — instale normalmente para o seu sistema operacional.)*

### Docker Desktop
Responsável por rodar o N8N em container, garantindo isolamento e portabilidade.

- Download: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
- **O Docker Desktop precisa estar aberto e rodando sempre que for usar o projeto.**

### Ngrok
Cria um túnel seguro entre a internet e o seu ambiente local, permitindo que webhooks externos alcancem o N8N.

- Download e conta gratuita: [https://ngrok.com](https://ngrok.com)
- Crie uma conta para obter seu token de autenticação.

---

## 📥 Clonagem do Projeto

Escolha uma pasta no seu computador onde deseja guardar o projeto. Abra o terminal nessa pasta e execute:

```bash
git clone https://github.com/LuterLT/ChatBotRestaurante
```

Depois, entre na pasta do projeto:

```bash
cd NOME_DO_PASTA_DO_PROJETO
```

---

## 📁 Estrutura do Projeto

```
.
├── docker-compose.yml
├── .env
├── .gitignore
├── README.md
├── chatbotTelegramRestaurante.json
└── n8n_data/
```

| Arquivo / Pasta | Descrição |
|---|---|
| `docker-compose.yml` | Define o container do N8N |
| `.env` | Variáveis de ambiente (não compartilhar) |
| `.gitignore` | Arquivos ignorados pelo Git |
| `README.md` | Este documento |
| `chatbotTelegramRestaurante.json` | Workflow exportado do N8N |
| `n8n_data/` | Dados persistidos do N8N |

---

## 🔐 Criação do .env

O arquivo `.env` armazena variáveis de configuração do projeto. Ele **nunca deve ser compartilhado ou enviado ao repositório** — por isso está listado no `.gitignore`.

Crie um arquivo chamado `.env` na raiz do projeto com o seguinte conteúdo:

```env
N8N_HOST=localhost
N8N_PORT=5678
TELEGRAM_TOKEN=ChaveDeAPIDoSeuBOT
WEBHOOK_URL=https://SEU_DOMINIO.ngrok-free.app

GENERIC_TIMEZONE=America/Sao_Paulo

N8N_BLOCK_ENV_ACCESS_IN_NODE=false
```

Substitua:
- `WEBHOOK_URL` pela URL gerada pelo Ngrok (veja a seção [Configuração do Ngrok](#-configuração-do-ngrok))

---

## 🐳 Criação do docker-compose.yml

O Docker Compose orquestra os containers definidos em um arquivo YAML. Ele garante que o N8N seja iniciado com as configurações corretas.


## 🚀 Iniciando o Projeto

Com o Docker Desktop aberto, execute na raiz do projeto:

```bash
docker compose up -d
```

A flag `-d` faz o container rodar em segundo plano. Para verificar se o container está ativo:

```bash
docker ps
```

O container `n8n` deve aparecer na lista com status `Up`.

---

## 🌐 Acessando o N8N

Abra o navegador e acesse:

```
http://localhost:5678
```

Na primeira vez, o N8N pedirá para criar uma conta de administrador. Preencha os dados e prossiga.

---

## 🔑 Chave de Licença Gratuita do N8N

Ao criar sua conta no N8N pela primeira vez, um **pop-up aparecerá oferecendo uma chave de licença gratuita**.

**Aceite essa oferta.** Após aceitar, você receberá um e-mail com a chave. Insira-a quando solicitado.

Essa licença libera funcionalidades adicionais da plataforma sem nenhum custo.

---

## 🌍 Configuração do Ngrok

### 1. Autenticação

Após criar sua conta no [ngrok.com](https://ngrok.com), acesse o painel e copie o seu **Auth Token**. Em seguida, execute:

```bash
ngrok config add-authtoken SEU_TOKEN
```

### 2. Iniciando o túnel

Com o N8N rodando na porta `5678`, execute:

```bash
ngrok http --domain=SeuDominioNgrok 5678
```

O Ngrok exibirá uma URL pública parecida com:

```
https://abc123.ngrok-free.app
```

Essa URL é o endereço público que aponta para o seu N8N local. **Copie-a — você vai precisar na próxima etapa.**

---

## 🔄 Configurando o WEBHOOK_URL

Abra o arquivo `.env` e cole a URL do Ngrok no campo `WEBHOOK_URL`:

```env
WEBHOOK_URL=https://abc123.ngrok-free.app
```

---

## 📤 Importando o Workflow

Ao rodar o projeto pela primeira vez (ou após clonar), importe o workflow do chatbot no N8N:

1. Acesse `http://localhost:5678`
2. No menu lateral, clique em **Workflows**
3. Clique em **Import from file**
4. Selecione o arquivo `chatbotTelegramRestaurante.json` da pasta do projeto
5. Confirme a importação

O workflow estará pronto para uso e edição.

---

## 💾 Salvando e Exportando o Workflow

O N8N **não salva automaticamente** o workflow no arquivo `.json` do projeto. Para manter o repositório atualizado, **toda vez que fizer alterações no workflow**, você deve exportá-lo manualmente e sobrescrever o arquivo anterior:

1. No N8N, abra o workflow `chatbotTelegramRestaurante`
2. Clique nos três pontinhos (menu do workflow) ou acesse **File > Download**
3. Salve o arquivo como `chatbotTelegramRestaurante.json` **dentro da pasta do projeto**, substituindo o arquivo existente
4. Em seguida, faça o commit e push normalmente (veja a seção [Git](#-git--enviando-alterações))

**Nunca envie alterações ao Git sem antes exportar e substituir o arquivo `.json`.**

---

## 🛠️ Comandos Úteis

| Comando | Descrição |
|---|---|
| `docker compose up -d` | Inicia os containers em segundo plano |
| `docker compose down` | Para e remove os containers |
| `docker compose restart` | Reinicia os containers |
| `docker logs -f n8n` | Exibe os logs em tempo real |
| `docker ps` | Lista containers em execução |

---

## 📤 Git — Enviando Alterações

Após exportar o workflow e fazer suas alterações, envie para o repositório:

```bash
git add .
git commit -m "descrição do que foi alterado"
git push origin main
```

**Lembre-se de avisar no grupo do WhatsApp que terminou a edição.**

---

## 🗂️ Persistência de Dados

A pasta `n8n_data/` armazena todos os dados internos do N8N: credenciais, execuções, configurações e workflows salvos na plataforma.

```
./n8n_data
```

Ela é mapeada pelo volume no `docker-compose.yml`. Isso significa que, mesmo que o container seja removido, os dados permanecem na máquina local.

---

## ⚠️ Observações Importantes

- **O Docker Desktop precisa estar aberto e em execução** para que os containers funcionem.
- **Nunca compartilhe o arquivo `.env`** — ele contém senhas e tokens sensíveis. Ele já está no `.gitignore` por padrão.
- **Apenas um integrante edita por vez.** Confirme no grupo do WhatsApp antes de começar.
- **Sempre dê `git fetch` e `git status` antes de editar** para garantir que está com a versão mais recente.
- **Sempre exporte o workflow** antes de fazer o commit, sobrescrevendo o arquivo `chatbotTelegramRestaurante.json`.