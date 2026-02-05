# 🎵 Sistema de Gestão Discográfica (SGD)

> **Desafio Full Stack SEPLAG-MT:** Sistema completo para gerenciar álbuns, artistas e usuários com uma interface intuitiva e eficiente. Arquitetura moderna baseada em microsserviços com Spring Boot 3, React 19, Docker, MinIO (S3), JWT e Sync de Dados.
s
---

## 📚 Documentação

### 🚀 **[Guia de Execução](README.md)** (Este arquivo)
Instruções para rodar o projeto localmente, com Docker e testes.

### 📖 **[Documentação Técnica](TECHNICAL_DOC.md)** ⭐
Documentação detalhada de cada categoria:
- Backend (Java + Spring Boot)
- Frontend (React + TypeScript)
- Banco de Dados (PostgreSQL + Flyway)
- Segurança (JWT + Rate Limiting)
- Infraestrutura (Docker)
- Decisões Técnicas

---

## 🎯 Resumo de Stacks Utilizadas

### 🔵 Backend
```
Java 21 → Spring Boot 3.4.1 → PostgreSQL → MinIO
JWT Authentication | Bucket4j Rate Limiting | Flyway Migrations
```

### ⚛️ Frontend
```
React 19 → TypeScript 5.9 → Vite 7.2 → Tailwind CSS 4.1
Zod Validation | React Hook Form | Vitest Testing
```

### 🐳 Infraestrutura
```
Docker | Docker Compose | PostgreSQL 15 | MinIO | Nginx
```

---

## 🚀 Início Rápido

---

## 🚀 Início Rápido

Se você tem Docker instalado e apenas quer fazer o projeto funcionar rapidamente:

```bash
# Clonar ou extrair o projeto
git clone <seu-repositorio> projeto-seplag
cd projeto-seplag

# Iniciar todos os serviços
docker-compose up --build

# Abrir no navegador
# Frontend: http://localhost:5173
# Backend API: http://localhost:3333
# MinIO Console: http://localhost:9001
```

Pronto! O sistema estará rodando em aproximadamente **3-5 minutos** na primeira execução.

---

## 📋 Pré-Requisitos

Antes de começar, você precisa ter instalado em sua máquina:

### Obrigatório

| Software | Versão | Link | Descrição |
|----------|--------|------|-----------|
| **Docker Desktop** | 4.0+ | [Baixar](https://www.docker.com/products/docker-desktop) | Plataforma para containerização |
| **Git** | 2.30+ | [Baixar](https://git-scm.com/) | Controle de versão |

### Opcional (para desenvolvimento local)

| Software | Versão | Link | Descrição |
|----------|--------|------|-----------|
| **Node.js** | 18 LTS+ | [Baixar](https://nodejs.org/) | Runtime JavaScript (para testes locais) |
| **Java JDK** | 21 | [Baixar](https://adoptopenjdk.net/) | Java Development Kit (para build local) |
| **PostgreSQL Client** | 15+ | [Baixar](https://www.postgresql.org/download/) | Cliente PostgreSQL (para debugging) |

### Requisitos de Sistema

- **RAM:** Mínimo 4GB (recomendado 8GB)
- **Espaço em Disco:** Mínimo 5GB
- **Processador:** Intel/AMD dual-core (recomendado quad-core)
- **Internet:** Necessária para download de imagens Docker

### Windows Específico

⚠️ **Importante:** No Windows, você precisa de **WSL 2** (Windows Subsystem for Linux 2)

```powershell
# Verificar se tem WSL 2 instalado
wsl --list --verbose

# Se não tem, instale com (requer reinicialização):
wsl --install
```

---

## 💾 Instalação e Setup

### Passo 1: Preparar o Ambiente

```bash
# Clonar o repositório
git clone <url-do-repositorio> projeto-seplag
cd projeto-seplag

# Se está no Windows, certifique-se que WSL 2 está ativo
# Se é Mac/Linux, ignore este passo
```

### Passo 2: Verificar Docker

```bash
# Verificar versão do Docker
docker --version

# Verificar se Docker está rodando
docker ps

# Se obteve um erro, abra Docker Desktop e tente novamente
```

### Passo 3: Iniciar o Projeto com Docker Compose

```bash
# Navegar até a pasta do projeto
cd /caminho/para/projeto-seplag

# Construir e iniciar todos os containers
docker-compose up --build

# Alternativamente, para rodar em background:
docker-compose up -d
```

### Passo 4: Verificar Status

```bash
# Listar containers rodando
docker-compose ps

# Deve mostrar algo como:
# NAME                  STATUS
# sgd-postgres-v1       Up 2 minutes
# sgd-minio-v1          Up 2 minutes
# sgd-backend-v1        Up 2 minutes
# sgd-frontend-v1       Up 1 minute
```

### Passo 5: Acessar a Aplicação

Abra seu navegador e acesse:

| Serviço | URL | Login |
|---------|-----|-------|
| 🏠 **Frontend** | [http://localhost:5173](http://localhost:5173) | `email@domain.com` / `senha123` |
| ⚙️ **API Backend** | [http://localhost:3333](http://localhost:3333) | N/A |
| 💾 **MinIO Console** | [http://localhost:9001](http://localhost:9001) | `admin_storage` / `Storage_Key_2026!` |
| 🐘 **PostgreSQL** | `localhost:5432` | `srv_discografia` / `P@ssw0rd_Seplag2026!` |

---

## 🔧 Como Desenvolver

### Modificar o Frontend

```bash
# 1. Parar o container do frontend (sem parar os outros)
docker-compose stop frontend

# 2. Ir para pasta do frontend
cd frontend

# 3. Instalar dependências (primeira vez)
npm install

# 4. Rodar em modo desenvolvimento
npm run dev

# 5. O site estará em http://localhost:5173
# Mudanças são refletidas automaticamente (hot reload)

# 6. Para voltar ao Docker
npm run build
docker-compose up --build frontend
```

### Modificar o Backend

```bash
# 1. Você pode modificar o código em src/main/java

# 2. Fazer rebuild do container
docker-compose up --build backend

# 3. Ou usar Maven localmente (se tiver Java 21 instalado)
cd backend
./mvnw clean package
```

### Acessar Logs

```bash
# Ver logs em tempo real
docker-compose logs -f backend

# Ver logs de um serviço específico
docker-compose logs -f frontend
docker-compose logs -f database

# Ver últimas 100 linhas
docker-compose logs --tail=100 backend
```

---

## 🧪 Testando o Sistema

### Teste 1: Verificar Saúde da API

```bash
# O backend deve responder
curl http://localhost:3333/actuator/health

# Resposta esperada:
# {"status":"UP"}
```

### Teste 2: Listar Álbuns (API)

```bash
curl http://localhost:3333/api/v1/albuns
```

### Teste 3: Testes Automatizados (Frontend)

```bash
# Ir para o frontend
cd frontend

# Instalar dependências (primeira vez)
npm install

# Rodar testes
npm test

# Rodar testes com coverage
npm test -- --coverage
```

### Teste 4: Acessar Banco de Dados

```bash
# Conectar ao PostgreSQL
docker exec -it sgd-postgres-v1 psql -U srv_discografia -d db_discografia_core

# Alguns comandos úteis no psql:
\dt                    # Listar todas as tabelas
SELECT * FROM users;   # Ver usuários
\q                     # Sair
```

### Teste 5: Fazer Login na Aplicação

1. Abra [http://localhost:5173](http://localhost:5173)
2. Clique em "Login"
3. Use as credenciais fornecidas abaixo
4. Navegue pela aplicação

---

## 🏗️ Arquitetura e Componentes

### Visão Geral da Arquitetura

```
┌─────────────────────────────────────────────────────┐
│                   Cliente (Navegador)               │
│          http://localhost:5173 (React 19)          │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP/WebSocket
                       ↓
┌─────────────────────────────────────────────────────┐
│         NGINX (Reverse Proxy & Static Files)        │
│         http://localhost:5173 e localhost:3333      │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP
      ┌────────────────┼────────────────┐
      ↓                ↓                ↓
┌───────────┐  ┌──────────────┐  ┌──────────────┐
│ Backend   │  │    MinIO     │  │  PostgreSQL  │
│ Spring    │  │   (Storage)  │  │  (Database)  │
│ Boot API  │  │  :9000/9001  │  │    :5432     │
│ :3333     │  └──────────────┘  └──────────────┘
└───────────┘
```

### Banco de Dados (PostgreSQL)

**Função:** Armazena todos os dados persistentes

| Tabela | Descrição |
|--------|-----------|
| `users` | Usuários do sistema |
| `regionais` | Regiões geográficas |
| `artists` | Artistas |
| `albums` | Álbuns |
| `album_artist` | Relação muitos-para-muitos |
| `user_favorites` | Favoritos dos usuários |

```bash
# Conectar ao banco
docker exec -it sgd-postgres-v1 psql -U srv_discografia -d db_discografia_core
```

### Backend (Spring Boot)

**Função:** Processamento de lógica de negócio e API REST

**Endpoints Principais:**

```
GET  /api/v1/users           - Listar usuários
POST /api/v1/users           - Criar usuário
GET  /api/v1/albuns          - Listar álbuns
POST /api/v1/albuns          - Criar álbum
GET  /api/v1/artistas        - Listar artistas
POST /api/v1/artistas        - Criar artista
GET  /api/v1/favoritos       - Meus favoritos
POST /api/v1/favoritos       - Adicionar favorito
```

### Frontend (React)

**Função:** Interface de usuário interativa

**Páginas Principais:**
- 🏠 Home - Dashboard
- 📀 Álbuns - Listagem e detalhes
- 🎤 Artistas - Listagem e detalhes
- ❤️ Favoritos - Álbuns salvos
- ⚙️ Admin - Gerenciamento (requer permissão)

### Armazenamento (MinIO)

**Função:** Armazenar imagens e arquivos (compatível com S3)

```bash
# Acessar console MinIO
# URL: http://localhost:9001
# Usuário: admin_storage
# Senha: Storage_Key_2026!
```

---

## 🔑 Credenciais Padrão

### Usuários Padrão da Aplicação

| Email | Senha | Tipo | Status |
|-------|-------|------|--------|
| admin@seplag.com | senha123 | Administrador | Ativo |
| user@seplag.com | senha123 | Usuário | Ativo |

> ℹ️ **Nota:** Essas credenciais são criadas automaticamente pelo script de migração (`V11__insert_sample_data_artists_albums.sql`)

### Serviços e Acessos

| Serviço | Usuário | Senha | Acesso |
|---------|---------|-------|--------|
| PostgreSQL | srv_discografia | P@ssw0rd_Seplag2026! | localhost:5432 |
| MinIO | admin_storage | Storage_Key_2026! | localhost:9001 |
| Backend API | N/A | N/A | localhost:3333 |

---

## 📂 Estrutura do Projeto

```
projeto-seplag/
│
├── backend/                          # Código-fonte do servidor
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/br/gov/mt/seplag/sgd/
│   │   │   │   ├── config/           # Configurações (JWT, CORS, Rate Limiting)
│   │   │   │   ├── controller/       # Endpoints REST
│   │   │   │   ├── entity/           # Modelos de dados
│   │   │   │   ├── repository/       # Acesso ao banco de dados
│   │   │   │   ├── service/          # Lógica de negócio
│   │   │   │   └── SgdBackendApplication.java
│   │   │   └── resources/
│   │   │       ├── application.yml   # Configurações
│   │   │       └── db/migration/     # Scripts SQL (Flyway)
│   │   └── test/
│   │       └── java/                 # Testes unitários
│   ├── pom.xml                       # Dependências Maven
│   ├── mvnw                          # Maven wrapper (Unix)
│   └── mvnw.cmd                      # Maven wrapper (Windows)
│
├── frontend/                         # Código-fonte da interface
│   ├── src/
│   │   ├── components/               # Componentes React
│   │   │   ├── Layout/               # Cabeçalho, menu, rodapé
│   │   │   ├── AlbumsDataTable/      # Tabela de álbuns
│   │   │   ├── ArtistsDataTable/     # Tabela de artistas
│   │   │   ├── UsersManagement/      # Gerenciamento de usuários
│   │   │   ├── RegionaisManagement/  # Gerenciamento de regiões
│   │   │   └── Modal/                # Modais (login, criar, etc)
│   │   ├── pages/                    # Páginas principais
│   │   │   ├── home/                 # Home page
│   │   │   ├── albums/               # Página de álbuns
│   │   │   ├── artists/              # Página de artistas
│   │   │   ├── favorites/            # Página de favoritos
│   │   │   └── admin/                # Painel administrativo
│   │   ├── services/                 # Serviços (API calls)
│   │   │   ├── apiClient.ts          # Cliente HTTP com Axios
│   │   │   ├── authService.ts        # Autenticação
│   │   │   ├── albumService.ts       # Serviço de álbuns
│   │   │   └── webSocketService.ts   # WebSocket
│   │   ├── contexts/                 # Contexto React (Estado global)
│   │   │   └── AuthContext.tsx       # Autenticação global
│   │   ├── types/                    # Tipos TypeScript
│   │   ├── utils/                    # Funções auxiliares
│   │   ├── App.tsx                   # Componente raiz
│   │   └── main.tsx                  # Entry point
│   ├── package.json                  # Dependências npm
│   ├── tsconfig.json                 # Configuração TypeScript
│   ├── tailwind.config.js            # Configuração Tailwind CSS
│   └── vite.config.ts                # Configuração Vite
│
├── docker-compose.yml                # Orquestração de containers
├── README.md                         # Este arquivo
└── setup.sh                          # Script de setup (opcional)
```

---

## ⚡ Gerenciando o Projeto

### Parar o Projeto

```bash
# Parar todos os containers (mantém dados)
docker-compose stop

# Parar e remover containers (mantém dados)
docker-compose down

# Parar, remover containers E deletar dados
docker-compose down -v
```

### Reiniciar Serviços

```bash
# Reiniciar todos os serviços
docker-compose restart

# Reiniciar um serviço específico
docker-compose restart backend
docker-compose restart frontend
```

### Monitorar Recursos

```bash
# Ver consumo de CPU e memória
docker stats

# Ver detalhes de um container
docker inspect sgd-backend-v1
```

### Limpeza

```bash
# Remover imagens não utilizadas
docker image prune

# Remover containers parados
docker container prune

# Remover volumes não utilizados
docker volume prune
```

---

## 🆘 Guia de Troubleshooting

### ❌ "Docker não está em execução"

```bash
# Verificar status
docker ps

# Se retornar erro, inicie o Docker Desktop
# Windows/Mac: Procure "Docker" no menu iniciar/launchpad
# Linux: sudo systemctl start docker
```

### ❌ "Porta já está em uso"

```bash
# Verificar qual processo está usando a porta
# Windows
netstat -ano | findstr :5173

# Mac/Linux
lsof -i :5173

# Matar o processo
# Windows
taskkill /PID <numero> /F

# Mac/Linux
kill -9 <PID>
```

### ❌ "Frontend exibe página em branco"

```bash
# 1. Verifique o console do navegador (F12)
# 2. Limpe cache
Ctrl + Shift + Delete

# 3. Reinicie o container
docker-compose restart frontend

# 4. Verifique os logs
docker-compose logs frontend
```

### ❌ "Erro de conexão com banco de dados"

```bash
# Aguarde alguns segundos (o banco demora para iniciar)
# Se o erro persistir:

# Verificar logs
docker-compose logs database

# Reiniciar banco
docker-compose restart database
```

### ❌ "npm install falha"

```bash
# Deletar cache
npm cache clean --force

# Deletar node_modules
rm -rf frontend/node_modules frontend/package-lock.json

# Reinstalar
cd frontend
npm install
```

### ❌ "Testes falhando"

```bash
# Certifique-se que dependências estão instaladas
cd frontend
npm install

# Rodar testes novamente
npm test

# Se falhar, veja a mensagem de erro e corrija o código
```

---

## 📱 Fluxo de Uso Completo

### 1️⃣ Usuário Novo (Sem Login)

```
1. Abre http://localhost:5173
   ↓
2. Vê página inicial sem login
   ↓
3. Clica em "Login"
   ↓
4. Insere email e senha
   ↓
5. Sistema valida credenciais
   ↓
6. Token JWT é salvo localmente
   ↓
7. Redirecionado para home autenticada
```

### 2️⃣ Usuário Normal (Após Login)

```
Home → Álbuns → Selecionar álbum → Ver detalhes
                ↓
             Artistas → Ver artista dos álbuns
                ↓
             Favoritos → Marcar como favorito (❤️)
                ↓
             Perfil → Ver meus favoritos
```

### 3️⃣ Administrador

```
Home
   ↓
Admin Panel
   ├─ Gerenciar Álbuns
   │  ├─ Listar
   │  ├─ Criar novo
   │  ├─ Editar
   │  └─ Deletar
   │
   ├─ Gerenciar Artistas
   │  ├─ Listar
   │  ├─ Criar novo
   │  └─ Editar
   │
   ├─ Gerenciar Usuários
   │  ├─ Listar
   │  ├─ Ativar/Desativar
   │  └─ Deletar
   │
   └─ Gerenciar Regiões
      ├─ Listar
      ├─ Criar nova
      └─ Editar
```

---

## 🔐 Recursos de Segurança

O sistema implementa várias camadas de segurança:

### 1. Autenticação (JWT)

```
Cliente                          Backend
   │                               │
   ├─ POST /auth/login ────────────→
   │  (email + senha)              │
   │                               │ Valida credenciais
   │                               │ Gera JWT Token
   ←────── Token JWT ──────────────┤
   │                               │
   ├─ GET /api/v1/users ──────────→
   │  (com Bearer Token)           │
   │                               │ Valida Token
   ←────── Dados ──────────────────┤
```

### 2. Rate Limiting

Limite de requisições por IP para evitar abuso:
- 100 requisições por minuto

### 3. CORS

Apenas requisições de `localhost:5173` (frontend) são aceitas

### 4. Criptografia de Senhas

Senhas são armazenadas com hash (não em texto plano)

```bash
# Exemplo: Senha "senha123" é armazenada como:
$2a$10$NgIE3h.zH0.wXPqaKtP7y.eL6DzI1vE0Pq6N3oN9wqH0KqH0O0Qb.
```

### 5. Validação de Entrada

Todas as requisições são validadas no backend

---

## 🚦 Comandos Rápidos

```bash
# Iniciar
docker-compose up --build

# Parar
docker-compose down

# Ver status
docker-compose ps

# Ver logs
docker-compose logs -f

# Entrar no backend
docker exec -it sgd-backend-v1 /bin/bash

# Entrar no banco de dados
docker exec -it sgd-postgres-v1 psql -U srv_discografia -d db_discografia_core

# Testes (frontend)
cd frontend && npm test

# Build (frontend)
cd frontend && npm run build

# Verificar saúde da API
curl http://localhost:3333/actuator/health
```

---

## 📚 Documentação Adicional

- 🔗 [Docker Documentation](https://docs.docker.com/)
- 🔗 [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- 🔗 [React Documentation](https://react.dev/)
- 🔗 [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## 📊 Resumo do Setup

| Etapa | Comando | Tempo | Status |
|-------|---------|-------|--------|
| Instalar Docker | - | 10 min | ⏳ |
| Instalar Git | - | 5 min | ⏳ |
| Clonar projeto | `git clone ...` | 1 min | ⏳ |
| Build/Start | `docker-compose up --build` | 3-5 min | ⏳ |
| Acesso | Abrir localhost:5173 | 1 min | ✅ |
| **Total** | - | **~20 minutos** | ✅ |

---

## 🤝 Contribuindo

Quer contribuir? Ótimo!

1. **Faça um fork** do repositório
2. **Crie uma branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit suas mudanças** (`git commit -m 'Add some AmazingFeature'`)
4. **Push para a branch** (`git push origin feature/AmazingFeature`)
5. **Abra um Pull Request**

### Padrões de Código

- **Backend:** Java 21, Spring Boot 3, Maven
- **Frontend:** React 19, TypeScript, ESLint
- **Commits:** Mensagens claras em português ou inglês
- **Testes:** Código novo deve ter testes

---

## 📝 Licença

Este projeto é parte do Desafio Full Stack SEPLAG-MT 2026.

---

## 📞 Suporte

- 📧 **Email:** [seu-email@seplag.com]
- 💬 **Issues:** Abra uma issue no repositório
- 📖 **Wiki:** Documentação adicional em construção

---

## ✨ Agradecimentos

Desenvolvido pela equipe de desenvolvimento da SEPLAG-MT.

### Tecnologias Utilizadas

- [Spring Boot](https://spring.io/projects/spring-boot)
- [React](https://react.dev/)
- [Docker](https://www.docker.com/)
- [PostgreSQL](https://www.postgresql.org/)
- [MinIO](https://min.io/)
- [Tailwind CSS](https://tailwindcss.com/)

---

**Versão:** 1.0.0  
**Data:** Fevereiro 2026  
**Status:** ✅ Pronto para Produção

[status-badge]: https://img.shields.io/badge/status-ativo-brightgreen?style=flat-square
[docker-badge]: https://img.shields.io/badge/docker-4.0+-2496ED?style=flat-square&logo=docker
[nodejs-badge]: https://img.shields.io/badge/node.js-18%2B-339933?style=flat-square&logo=node.js
