# 🎵 Sistema de Gestão Discográfica (SGD)

**Desafio Full Stack SEPLAG-MT:** Sistema completo para gerenciar álbuns, artistas e usuários com uma interface intuitiva e eficiente. Arquitetura moderna baseada em microsserviços com Spring Boot 3, React 19, Docker, MinIO (S3), JWT e Sync de Dados.

---

## 📋 Sumário

- [Visão Geral](#visão-geral)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Como Rodar o Projeto](#como-rodar-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Endpoints da API](#endpoints-da-api)
- [Autenticação](#autenticação)
- [Dados de Acesso](#dados-de-acesso)

---

## 🎯 Visão Geral

O SGD é uma aplicação full-stack para gerenciar discografias de artistas. A aplicação permite:

- **Gerenciamento de Artistas:** Criar, editar, visualizar e deletar artistas com imagens
- **Gerenciamento de Álbuns:** Criar, editar, visualizar e deletar álbuns associados aos artistas
- **Gerenciamento de Usuários:** Controlar permissões e roles (ADMIN/USER)
- **Gerenciamento de Regionais:** Sincronizar regionais com API externa
- **Favoritos:** Marcar e desmarcar álbuns como favoritos
- **Autenticação Segura:** Sistema JWT com refresh tokens
- **Armazenamento de Mídia:** Usar MinIO (S3-compatible) para armazenar capas de álbuns
- **Sincronização de Dados:** Sincronização automática de regionais com API externa

---

## 🛠️ Tecnologias Utilizadas

### Backend
- **Java 21** com Spring Boot 3.4.1
- **Spring Data JPA** para acesso a dados
- **Spring Security** com JWT
- **PostgreSQL 15** como banco de dados
- **Flyway** para migrations
- **Lombok** para reduzir boilerplate
- **Swagger/OpenAPI** para documentação da API

### Frontend
- **React 19** com TypeScript
- **React Router DOM** para roteamento
- **Tailwind CSS** para estilização
- **PrimeReact** para componentes UI avançados
- **React Hook Form** com Zod para validação
- **Axios** para requisições HTTP
- **WebSocket/STOMP** para comunicação em tempo real
- **Vitest** para testes unitários

### Infraestrutura
- **Docker & Docker Compose** para containerização
- **MinIO** para armazenamento de objetos (S3-compatible)
- **Nginx** como reverse proxy para o frontend

---

## 🚀 Como Rodar o Projeto

### Opção 1: Com Docker (Recomendado) ⭐

#### Pré-requisitos
- Docker instalado
- Docker Compose instalado
- Git

#### Passos

1. **Clone o repositório e entre no diretório:**
```bash
cd projeto-seplag/joaosantos070197
```

2. **Execute o script de setup:**

**No Linux/Mac:**
```bash
chmod +x setup.sh
./setup.sh
```

**No Windows (PowerShell):**
```powershell
bash setup.sh
```

3. **Aguarde a inicialização (cerca de 30-60 segundos)**

4. **Acesse os serviços:**
- 🌐 **Frontend**: http://localhost:5173
- 🔧 **Backend API**: http://localhost:3333
- 📊 **Swagger UI**: http://localhost:3333/swagger-ui/index.html
- 💾 **MinIO Console**: http://localhost:9001

#### Dados de Acesso do MinIO
- **Usuário**: `admin_storage`
- **Senha**: `Storage_Key_2026!`

---

### Opção 2: Sem Docker (Desenvolvimento Local)

#### Pré-requisitos
- Java 21 JDK
- Node.js 18+ com npm
- PostgreSQL 15
- Git

#### Passos

##### 1. Preparar Banco de Dados PostgreSQL

```sql
CREATE USER srv_discografia WITH PASSWORD 'P@ssw0rd_Seplag2026!';
CREATE DATABASE db_discografia_core OWNER srv_discografia;
GRANT ALL PRIVILEGES ON DATABASE db_discografia_core TO srv_discografia;
```

##### 2. Configurar Backend

```bash
cd backend

# Compilar o projeto
./mvnw clean package -DskipTests

# Rodar o backend
./mvnw spring-boot:run
```

O backend estará disponível em: **http://localhost:3333**

##### 3. Configurar Frontend

```bash
cd ../frontend

# Instalar dependências
npm install

# Executar em modo desenvolvimento
npm run dev
```

O frontend estará disponível em: **http://localhost:5173**

---

## ✨ Funcionalidades

### Para Usuários Autenticados (USER e ADMIN)

#### 🏠 Home Page
- Dashboard com estatísticas gerais
- Total de artistas
- Total de álbuns
- Layout responsivo e intuitivo

#### 🎤 Gerenciamento de Artistas
- **Listar**: Visualizar todos os artistas com paginação
- **Buscar**: Filtrar artistas por nome
- **Criar**: Adicionar novo artista com foto
- **Editar**: Modificar dados e foto do artista
- **Deletar**: Remover artista do sistema
- **Upload de Imagem**: Suporte a imagens em MinIO

#### 💿 Gerenciamento de Álbuns
- **Listar**: Visualizar álbuns com paginação
- **Buscar**: Filtrar por título ou artista
- **Criar**: Adicionar novo álbum com capa
- **Editar**: Modificar dados e capa do álbum
- **Deletar**: Remover álbum
- **Favoritar**: Marcar/desmarcar como favorito
- **Upload de Capa**: Suporte a imagens em MinIO

#### ❤️ Página de Favoritos
- Visualizar todos os álbuns favoritados
- Remover de favoritos
- Acesso rápido aos álbuns preferidos

### Para Administradores (ADMIN) 👨‍💼

#### 👥 Gerenciamento de Usuários
- Listar todos os usuários cadastrados
- Alterar roles (ADMIN/USER)
- Visualizar informações detalhadas de usuários
- Controle de acesso granular

#### 📍 Gerenciamento de Regionais
- Listar regionais disponíveis
- Sincronizar regionais com API externa (https://integrador-argus-api.geia.vip/)
- Ativar/inativar regionais
- Adicionar regionais manualmente
- Histórico de sincronização

---

## 📊 Estrutura do Projeto

```
projeto-seplag/joaosantos070197/
├── backend/                    # API Spring Boot
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/br/gov/mt/seplag/sgd/
│   │   │   │   ├── controller/      # Controllers REST
│   │   │   │   ├── service/         # Lógica de negócio
│   │   │   │   ├── repository/      # Acesso a dados
│   │   │   │   ├── entity/          # Entidades JPA
│   │   │   │   ├── dto/             # Data Transfer Objects
│   │   │   │   ├── config/          # Configurações
│   │   │   │   └── exception/       # Tratamento de exceções
│   │   │   └── resources/
│   │   │       ├── db/migration/    # Scripts Flyway
│   │   │       ├── application.yml  # Configurações
│   │   │       └── application.properties
│   │   └── test/                    # Testes unitários
│   ├── pom.xml                # Dependências Maven
│   ├── mvnw                   # Maven wrapper
│   └── Dockerfile             # Imagem Docker
│
├── frontend/                  # Aplicação React
│   ├── src/
│   │   ├── components/        # Componentes React
│   │   │   ├── Layout/        # Layout principal
│   │   │   ├── AlbumsDataTable/
│   │   │   ├── ArtistsDataTable/
│   │   │   ├── UsersManagement/
│   │   │   ├── RegionaisManagement/
│   │   │   └── ui/            # Componentes UI
│   │   ├── pages/             # Páginas da aplicação
│   │   │   ├── home/
│   │   │   ├── albums/
│   │   │   ├── artists/
│   │   │   ├── favorites/
│   │   │   └── admin/
│   │   ├── services/          # Serviços (API, Auth, etc)
│   │   ├── contexts/          # Context API
│   │   ├── types/             # Tipos TypeScript
│   │   ├── utils/             # Utilitários
│   │   ├── App.tsx            # Componente raiz
│   │   └── main.tsx           # Entry point
│   ├── package.json           # Dependências npm
│   ├── vite.config.ts         # Configuração Vite
│   ├── tailwind.config.js     # Configuração Tailwind
│   ├── tsconfig.json          # Configuração TypeScript
│   ├── Dockerfile             # Imagem Docker
│   └── nginx.conf             # Configuração Nginx
│
├── docker-compose.yml         # Orquestração de containers
├── setup.sh                   # Script de inicialização
└── README.md                  # Este arquivo
```

---

## 🔌 Endpoints da API

### Autenticação
```
POST   /auth/login             # Fazer login
POST   /auth/register          # Registrar novo usuário
POST   /auth/refresh           # Renovar token JWT
```

### Artistas
```
GET    /api/v1/artists                    # Listar artistas (com paginação)
GET    /api/v1/artists/{id}               # Buscar artista por ID
POST   /api/v1/artists                    # Criar novo artista
PUT    /api/v1/artists/{id}               # Editar artista
DELETE /api/v1/artists/{id}               # Deletar artista
```
**Query Parameters**: `name` (filtro), `page`, `size`, `sort`

### Álbuns
```
GET    /api/v1/albums                     # Listar álbuns (com paginação)
GET    /api/v1/albums/{id}                # Buscar álbum por ID
POST   /api/v1/albums                     # Criar novo álbum
PUT    /api/v1/albums/{id}                # Editar álbum
DELETE /api/v1/albums/{id}                # Deletar álbum
POST   /api/v1/albums/{id}/favorite       # Favoritar álbum
DELETE /api/v1/albums/{id}/favorite       # Desfavoritar álbum
```
**Query Parameters**: `artistId`, `title`, `page`, `size`, `sort`

### Usuários
```
GET    /api/users              # Listar usuários
GET    /api/users/{id}         # Buscar usuário por ID
PUT    /api/users/{id}/role    # Alterar role do usuário
```

### Regionais
```
GET    /api/v1/regionais                  # Listar regionais
GET    /api/v1/regionais/{id}             # Buscar regional por ID
POST   /api/v1/regionais                  # Criar regional
POST   /api/v1/regionais/sync             # Sincronizar com API externa
PATCH  /api/v1/regionais/{id}/status      # Alterar status da regional
```

### Estatísticas
```
GET    /api/v1/stats          # Obter estatísticas gerais
```

---

## 🔐 Autenticação

O sistema utiliza **JWT (JSON Web Tokens)** para autenticação:

1. **Login**: Envie credenciais para `/auth/login`
2. **Access Token**: Receba um token JWT com 15 minutos de validade
3. **Refresh Token**: Use para renovar o access token
4. **Bearer Token**: Inclua no header de requisições protegidas:
   ```
   Authorization: Bearer <seu_token_jwt>
   ```

### Exemplo de Login
```bash
curl -X POST http://localhost:3333/auth/login \
  -H "Content-Type: application/json" \
  -d '{"login": "seu_usuario", "senhaLogin": "sua_senha"}'
```

### Roles de Acesso
- **USER**: Acesso a artistas, álbuns e favoritos
- **ADMIN**: Acesso completo + gerenciamento de usuários e regionais

---

## 📝 Dados de Acesso

### Banco de Dados PostgreSQL
- **Host**: localhost (desenvolvimento) / database (Docker)
- **Porta**: 5432
- **Usuário**: `srv_discografia`
- **Senha**: `P@ssw0rd_Seplag2026!`
- **Database**: `db_discografia_core`

### MinIO (Object Storage)
- **Console**: http://localhost:9001
- **Usuário**: `admin_storage`
- **Senha**: `Storage_Key_2026!`
- **Bucket**: `capas-albuns-media` (criado automaticamente)
- **API**: http://localhost:9000

### Dados Padrão
O sistema vem com alguns dados pré-configurados após a primeira exécução. Verifique o banco de dados ou faça login para explorar.

---

## 🧪 Testes

### Backend
```bash
cd backend
./mvnw test
```

### Frontend
```bash
cd frontend
npm run test
```

---

## 📚 Documentação da API

A documentação interativa da API está disponível via Swagger UI quando o backend está rodando:

**http://localhost:3333/swagger-ui/index.html**

---

## 🛠️ Comandos Úteis para Desenvolvimento

### Parar os containers Docker
```bash
docker-compose down
```

### Remover volumes (limpar dados)
```bash
docker-compose down -v
```

### Ver logs dos serviços
```bash
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f database
```

### Rebuild dos containers
```bash
docker-compose down
docker-compose up --build -d
```

### Acessar terminal do container
```bash
docker-compose exec backend bash
docker-compose exec frontend sh
docker-compose exec database psql -U srv_discografia -d db_discografia_core
```

---

## ❓ Troubleshooting

### Portas já estão em uso
```bash
# Verifique quais portas estão em uso
lsof -i :5173  # Frontend
lsof -i :3333  # Backend
lsof -i :5432  # Database
lsof -i :9000  # MinIO API
lsof -i :9001  # MinIO Console
```

### Container não inicia
```bash
# Limpar tudo e recomeçar
docker-compose down -v
docker-compose up --build -d
```

### Problemas com banco de dados
```bash
# Verificar logs do PostgreSQL
docker-compose logs database

# Reconectar ao banco
docker-compose exec database psql -U srv_discografia -d db_discografia_core
```

### Frontend não carrega
```bash
# Limpar cache e reinstalar dependências
cd frontend
rm -rf node_modules package-lock.json
npm install
npm run dev
```

---

## 📞 Suporte

Para dúvidas ou problemas:

1. Verifique os logs: `docker-compose logs`
2. Verifique se todas as portas estão disponíveis
3. Certifique-se que Docker está instalado e rodando
4. Verifique a documentação Swagger: http://localhost:3333/swagger-ui/
5. Verifique o console do navegador para erros de frontend

---

## 📄 Licença

Sistema desenvolvido como Desafio Full Stack SEPLAG-MT 2026.

---

## 👨‍💻 Desenvolvedor

**João Santos** - joaosantos070197

Data: Fevereiro de 2026

