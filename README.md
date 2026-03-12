# 📅 Agendador de Tarefas — Projeto de Portfólio

Sistema de agendamento de tarefas com envio de notificações por e-mail, desenvolvido com arquitetura de microsserviços em Java Spring Boot.

---

## 🏗️ Arquitetura

O projeto é composto por 4 microsserviços que se comunicam via Feign Client:

| Serviço | Porta | Descrição |
|---|---|---|
| **bff-agendador** | 8083 | Gateway de entrada, orquestra os demais serviços |
| **usuario** | 8080 | Gerenciamento de usuários e autenticação JWT |
| **tarefas** | 8081 | CRUD de tarefas e agendamentos |
| **notificacao** | 8082 | Envio de e-mails via SMTP |

### Bancos de dados
- **PostgreSQL** — dados de usuários (porta 5434)
- **MongoDB** — dados de tarefas (porta 27017)

---

## 🚀 Como rodar o projeto

### Pré-requisitos
- [Docker](https://docs.docker.com/get-docker/) instalado
- [Docker Compose](https://docs.docker.com/compose/install/) instalado

> Não é necessário instalar Java, PostgreSQL ou MongoDB. O Docker cuida de tudo.

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/felcross/Infra.git
cd Infra
```

**2. Configure as variáveis de ambiente**

Copie o arquivo de exemplo:
```bash
cp .env.example .env
```

Abra o `.env` e preencha **apenas** a senha do seu e-mail Gmail:
```env
JWT_SECRET=c3VhLWNoYXZlLXNlY3JldGEtc3VwZXItc2VndXJhLXF1ZS1kZXZlLXNlci1iZW0tbG9uZ2E=
DB_USERNAME=postgres
DB_PASSWORD=123
MONGO_URI=mongodb://mongodb:27017/db_agendador
MAIL_PASSWORD=          # cole aqui sua senha de app do Gmail
```

> ⚠️ **Senha do Gmail:** você precisa de uma **senha de aplicativo**, não a senha normal da conta.
> Gere em: [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)

**3. Suba todos os serviços**
```bash
docker compose up -d
```

Aguarde todos os containers subirem (pode levar 1-2 minutos na primeira vez).

**4. Verifique se está tudo rodando**
```bash
docker ps
```

Todos os containers devem estar com status `Up`.

---

## 📖 Documentação da API (Swagger)

Após subir o projeto, acesse a documentação interativa de cada serviço:

- **BFF (porta de entrada)** → http://localhost:8083/swagger-ui/index.html
- **Usuário** → http://localhost:8080/swagger-ui/index.html
- **Tarefas** → http://localhost:8081/swagger-ui/index.html
- **Notificação** → http://localhost:8082/swagger-ui/index.html

> 💡 **Recomendado:** use o Swagger do BFF como ponto de entrada principal, pois ele agrega todos os endpoints do sistema.

---

## 🔄 Fluxo de uso

1. **Cadastre um usuário** → `POST /usuario`
2. **Faça login** → `POST /usuario/login` e copie o token JWT retornado
3. **Autorize no Swagger** → clique no botão 🔒 **Authorize** e cole o token no formato `Bearer seu_token`
4. **Crie uma tarefa** → `POST /tarefas` com data/hora de agendamento
5. **Aguarde o e-mail** → o sistema verifica automaticamente a cada 30 segundos e envia notificação quando o horário da tarefa se aproxima

---

## 🛑 Como parar o projeto

```bash
docker compose down
```

> Os dados persistem nos volumes do Docker, então ao subir novamente com `docker compose up -d` tudo estará como deixou.

---

## 🛠️ Tecnologias utilizadas

- **Java 21**
- **Spring Boot 3**
- **Spring Security + JWT**
- **Spring Cloud OpenFeign**
- **PostgreSQL**
- **MongoDB**
- **Docker & Docker Compose**
- **Gradle**
- **Swagger / OpenAPI**
- **Thymeleaf** (templates de e-mail)
- **GitHub Actions** (CI)

---

## 📁 Estrutura do projeto

Infra/
├── docker-compose.yml
├── .env.example
├── .gitignore
└── README.md

Microsserviços (repositórios separados):
├── bff-agendador
├── notificacao
├── tarefas
└── usuario

