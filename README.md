# 🍽️ Chatbot de Restaurante via Telegram — N8N

Automação completa de atendimento para restaurantes no Telegram, construída no **N8N**. O bot gerencia pedidos, reservas de mesas, pagamentos via PIX (Mercado Pago) e um painel administrativo — tudo sem nenhuma linha de código além dos nós do workflow.

---

## ✨ Funcionalidades

### 🛒 Cardápio & Pedidos
- Menu interativo por categorias: **Entradas**, **Salgados**, **Pratos Principais** e **Promoções**
- Montagem de pedido com botões inline do Telegram
- Exibição do resumo do pedido antes de confirmar
- Envio do cardápio completo em PDF (via Google Drive)

### 💳 Pagamentos
- **PIX** integrado ao **Mercado Pago**: geração de link de pagamento e confirmação automática via webhook
- **Retirada no local**: opção sem pagamento online
- **Entrega**: coleta de endereço e confirmação do pedido

### 🪑 Reservas de Mesa
- Página web dedicada (`reserva.html`) com visual premium (dark mode, dourado)
- Consulta de disponibilidade em tempo real via webhook N8N
- Seleção de data, horário e número de pessoas
- Confirmação da reserva enviada direto no Telegram
- Suporte a múltiplas unidades do restaurante

### 🎧 Suporte ao Cliente
- Abertura de chamados com ID único
- Notificação automática ao atendente configurado
- Confirmação enviada ao usuário

### 🔐 Painel Administrativo
- Acesso restrito via Telegram ID autorizado
- Comandos via chat para:
  - `/pedido` — criar pedido manualmente
  - Consultar, atualizar e cancelar pedidos
  - Consultar, reservar e cancelar mesas
  - Preencher e consultar endereço de entrega
  - Acompanhar e marcar pedidos como entregues
- Fallback amigável para comandos inválidos
- Registro de chamados no **Google Sheets**

---

## 🧱 Stack & Integrações

| Tecnologia | Uso |
|---|---|
| **N8N** | Motor de automação / orquestração dos fluxos |
| **Telegram Bot API** | Interface de atendimento ao cliente e admin |
| **Mercado Pago API** | Geração e confirmação de pagamentos PIX |
| **Google Drive** | Armazenamento e envio do cardápio em PDF |
| **Google Sheets** | Registro de chamados de suporte |
| **Docker / Docker Compose** | Deploy do N8N em qualquer servidor |
| **Webhook (N8N)** | Integração com a página de reservas e confirmação de pagamento |

---

## 🗂️ Estrutura do Projeto

```
├── chatbotTelegramRestaurante.json   # Workflow N8N completo (133 nós)
├── reserva.html                      # Página web de reserva de mesas
├── docker-compose.yml                # Deploy do N8N via Docker
└── .env                              # Variáveis de ambiente (não versionado)
```

---

## 🚀 Como Usar

### 1. Pré-requisitos
- Docker e Docker Compose instalados
- Conta no Telegram e um **Bot Token** (via [@BotFather](https://t.me/BotFather))
- Conta no **Mercado Pago** com credenciais de API
- Conta Google com acesso ao Drive e Sheets

### 2. Configurar variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
TELEGRAM_TOKEN=seu_token_do_telegram
# Demais credenciais são configuradas dentro do N8N
```

### 3. Subir o N8N

```bash
docker-compose up -d
```

Acesse o N8N em `http://localhost:5678`.

### 4. Importar o Workflow

1. No N8N, vá em **Workflows → Import**
2. Selecione o arquivo `chatbotTelegramRestaurante.json`
3. Configure as credenciais: Telegram, Mercado Pago, Google Drive e Google Sheets

### 5. Hospedar a Página de Reservas

Sirva o arquivo `reserva.html` em qualquer hosting estático (GitHub Pages, Vercel, Netlify etc.) e atualize a URL do webhook no arquivo para apontar para a sua instância do N8N.

### 6. Configurar o Admin

No nó **Contador Global**, substitua o valor do array `atendentes` pelo seu Telegram ID:

```js
bancoDados.atendentes = ['SEU_TELEGRAM_ID'];
```

---

## ⚙️ Variáveis Configuráveis no Workflow

| Variável | Onde configurar | Descrição |
|---|---|---|
| `atendentes` | Nó *Contador Global* | IDs do Telegram dos admins |
| `promocaoAtiva` | Nó *Contador Global* | Ativa/desativa aba de promoções |
| `precos` | Nó *Calcular Total* | Tabela de preços do cardápio |
| URL da reserva | Nó *Enviar Link Reserva* | URL pública da `reserva.html` |
| Webhook MP | Nó *Webhook Confirmação MP* | Endpoint configurado no Mercado Pago |

---

## 📸 Fluxo Resumido

```
Usuário → Telegram Bot
              ├── Cardápio → Pedido → Endereço → Pagamento (PIX / Retirada)
              ├── Reservas → Página Web → Confirmação no Telegram
              ├── Suporte → Chamado → Notifica Atendente
              └── Admin (ID autorizado) → Comandos de gestão
```

---

## 📄 Licença

MIT — sinta-se livre para adaptar ao seu restaurante.