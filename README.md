# 📂 Guia de Estrutura do Projeto SGD

## Índice
1. [Visão Geral](#visão-geral)
2. [Estrutura de Pastas](#estrutura-de-pastas)
3. [Arquitetura do Projeto](#arquitetura-do-projeto)
4. [Stack de Tecnologias](#stack-de-tecnologias)
5. [Fluxo de Dados](#fluxo-de-dados)
6. [Como Contribuir](#como-contribuir)

---

## 🎯 Visão Geral

**SGD (Sistema de Gestão Discográfica)** é uma aplicação full-stack modern para gerenciar:
- 🎵 **Álbuns e Artistas**
- 👥 **Usuários e Permissões**
- 🌍 **Regionais/Localidades**
- 💾 **Armazenamento de Mídia** (MinIO S3)
- 🔐 **Autenticação e Autorização** (JWT)

**Arquitetura:** Microserviços com containerização Docker, REST API e WebSocket em tempo real.

---

## 📁 Estrutura de Pastas

```
projeto-seplag/
│
├── 📁 backend/                    # 🔵 API Spring Boot (Java 21)
│   ├── src/main/java/
│   │   └── br/gov/mt/seplag/sgd/
│   │       ├── config/            # ⚙️ Configurações da aplicação
│   │       ├── controller/        # 🎮 Endpoints REST
│   │       ├── service/           # 💼 Lógica de negócio
│   │       ├── repository/        # 🗄️ Acesso a dados (JPA)
│   │       ├── entity/            # 📊 Modelos de dados
│   │       ├── dto/               # 📝 Data Transfer Objects
│   │       ├── exception/         # ⚠️ Tratamento de erros
│   │       ├── security/          # 🔐 JWT e autenticação
│   │       ├── filter/            # 🔍 Filtros HTTP (Rate Limiting)
│   │       └── util/              # 🛠️ Utilitários
│   │
│   ├── src/main/resources/
│   │   ├── application.yml        # 📋 Config Spring
│   │   ├── application.properties # 🔧 Propriedades
│   │   └── db/migration/          # 🐘 Scripts SQL (Flyway)
│   │       ├── V1__init_schema.sql
│   │       ├── V2__create_table_users.sql
│   │       └── ...
│   │
│   ├── src/test/java/             # ✅ Testes unitários
│   │
│   ├── pom.xml                    # 📦 Dependências Maven
│   ├── mvnw & mvnw.cmd            # 🚀 Maven Wrapper
│   ├── dockerfile                 # 🐳 Imagem Docker
│   └── .gitignore
│
├── 📁 frontend/                   # ⚛️ App React (TypeScript)
│   ├── src/
│   │   ├── 📁 components/         # 🧩 Componentes reutilizáveis
│   │   │   ├── Layout/            # 📐 Layout principal
│   │   │   │   ├── Header/
│   │   │   │   ├── Sidebar/
│   │   │   │   ├── Footer/
│   │   │   │   └── Main/
│   │   │   ├── Modal/             # 🪟 Modais (Auth, Create)
│   │   │   ├── AlbumsDataTable/   # 📋 Tabelas
│   │   │   ├── ArtistsDataTable/
│   │   │   ├── UsersManagement/
│   │   │   ├── RegionaisManagement/
│   │   │   ├── NotificationCenter/ # 🔔 Notificações
│   │   │   ├── Loading/           # ⏳ Spinner
│   │   │   ├── ProtectedRoute.tsx # 🔐 Rota privada
│   │   │   └── ui/                # 🎨 Componentes base
│   │   │
│   │   ├── 📁 pages/              # 📄 Páginas (rotas)
│   │   │   ├── home/              # 🏠 Homepage
│   │   │   ├── albums/            # 🎵 Gerenciar álbuns
│   │   │   ├── artists/           # 🎤 Gerenciar artistas
│   │   │   ├── favorites/         # ❤️ Favoritos
│   │   │   └── admin/             # 👨‍💼 Painel admin
│   │   │       ├── UsersPage.tsx
│   │   │       └── RegionaisPage.tsx
│   │   │
│   │   ├── 📁 services/           # 🌐 Serviços HTTP
│   │   │   ├── api.ts             # 🔌 Instância do axios
│   │   │   ├── apiClient.ts       # 📡 Cliente HTTP estruturado
│   │   │   ├── authService.ts     # 🔐 Login/Logout
│   │   │   ├── albumService.ts    # 💿 CRUD Álbuns
│   │   │   ├── artistService.ts   # 🎤 CRUD Artistas
│   │   │   ├── favoriteService.ts # ❤️ Favoritos
│   │   │   ├── statsService.ts    # 📊 Estatísticas
│   │   │   └── webSocketService.ts # 🔗 WebSocket realtime
│   │   │
│   │   ├── 📁 contexts/           # 🔄 React Context
│   │   │   └── AuthContext.tsx     # 👤 Estado global auth
│   │   │
│   │   ├── 📁 types/              # 📘 TypeScript types
│   │   │   ├── api.types.ts       # 🔌 Tipos de API
│   │   │   ├── auth.types.ts      # 🔐 Tipos de auth
│   │   │   ├── models.ts          # 📊 Modelos de dados
│   │   │   └── zod.types.ts       # ✔️ Schemas Zod
│   │   │
│   │   ├── 📁 utils/              # 🛠️ Utilitários
│   │   │   └── tokenUtils.ts      # 🎟️ Gerenciamento de JWT
│   │   │
│   │   ├── 📁 routes/             # 🗺️ Configuração de rotas
│   │   │   └── menuRoutes.ts      # 📍 Menu de navegação
│   │   │
│   │   ├── 📁 __tests__/          # ✅ Testes Vitest
│   │   │   ├── App.test.tsx
│   │   │   ├── authService.test.ts
│   │   │   └── ...
│   │   │
│   │   ├── 📁 assets/             # 🖼️ Imagens e mídia
│   │   │
│   │   ├── App.tsx                # 🚀 Root component
│   │   ├── main.tsx               # 📍 Entry point
│   │   ├── index.css              # 🎨 Estilos globais
│   │   ├── App.css                # 🎨 Estilos do App
│   │   └── setupTests.ts          # ⚙️ Config testes
│   │
│   ├── public/                    # 🌐 Arquivos estáticos
│   ├── package.json               # 📦 Dependências NPM
│   ├── vite.config.ts             # ⚡ Config Vite
│   ├── tsconfig.json              # 📘 Config TypeScript
│   ├── tailwind.config.js         # 🎨 Config Tailwind
│   ├── postcss.config.cjs         # 🎨 Config PostCSS
│   ├── eslint.config.js           # 📝 Linting rules
│   ├── nginx.conf                 # 🌐 Config Nginx
│   ├── dockerfile                 # 🐳 Imagem Docker
│   └── .gitignore
│
├── 📁 .idea/                      # 💡 Config IntelliJ
├── 📄 docker-compose.yml          # 🐳 Orquestração de containers
├── 🔨 setup.sh                    # 🚀 Script de setup
├── 📄 README.md                   # 📖 Documentação principal
└── 📄 README_ESTRUTURA.md         # 📂 Este arquivo

```

---

## 🏗️ Arquitetura do Projeto

### Diagrama de Componentes

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENTE (Browser)                        │
│                   React 19 + TypeScript                      │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTP + WebSocket
┌─────────────────────────────────────────────────────────────┐
│                  FRONTEND (Nginx)                            │
│  ▪ Components (UI, Modals, Tables)                          │
│  ▪ Pages (Home, Albums, Artists, Admin)                    │
│  ▪ Services (API calls, WebSocket)                         │
│  ▪ Contexts (AuthContext para estado global)               │
│  ▪ Utils (Token management, validation)                    │
└──────────────────────┬──────────────────────────────────────┘
                       │ REST API (3333) + WS
┌─────────────────────────────────────────────────────────────┐
│                  BACKEND (Spring Boot)                       │
│  ▪ Controllers (Endpoints REST)                             │
│  ▪ Services (Business Logic)                                │
│  ▪ Repositories (Database Access)                           │
│  ▪ Security (JWT, Rate Limiting)                            │
│  ▪ WebSocket (Real-time updates)                            │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   ┌────▼─────┐  ┌────▼─────┐  ┌────▼─────┐
   │PostgreSQL│  │  MinIO   │  │ WebSocket│
   │  (5432)  │  │   (9000) │  │ (Server) │
   └──────────┘  └──────────┘  └──────────┘
```

---

## 🔧 Stack de Tecnologias

### Backend (Java)
| Camada | Tecnologia | Versão | Uso |
|--------|-----------|--------|-----|
| **Runtime** | Java JDK | 21 | Linguagem principal |
| **Framework** | Spring Boot | 3.4.1 | Framework web |
| **ORM** | Spring Data JPA | - | Acesso a dados |
| **Banco** | PostgreSQL | 15 | Banco de dados relacional |
| **Migrations** | Flyway | - | Versionamento de schema |
| **Segurança** | Spring Security + JWT | 4.4.0 | Autenticação |
| **Rate Limiting** | Bucket4j | 8.0.1 | Controle de requisições |
| **Storage** | MinIO SDK | 8.5.7 | Armazenamento de objetos |
| **Validation** | Spring Validation | - | Validação de dados |
| **WebSocket** | Spring WebSocket | - | Comunicação realtime |
| **API Docs** | SpringDoc OpenAPI | 2.8.3 | Documentação Swagger |
| **Build** | Maven | - | Gerenciador de dependências |

### Frontend (TypeScript)
| Camada | Tecnologia | Versão | Uso |
|--------|-----------|--------|-----|
| **Runtime** | Node.js | 18+ | Runtime JavaScript |
| **Framework** | React | 19.2 | Biblioteca UI |
| **Linguagem** | TypeScript | 5.9 | Type-safe JavaScript |
| **Build** | Vite | 7.2 | Bundler moderno |
| **Roteamento** | React Router | 7.12 | SPA routing |
| **Styling** | Tailwind CSS | 4.1 | Utility-first CSS |
| **HTTP Client** | Axios | 1.13 | Requisições HTTP |
| **Forms** | React Hook Form | 7.71 | Gerenciamento de forms |
| **Validação** | Zod | 4.3 | Schema validation |
| **Icons** | Lucide + React Icons | - | Biblioteca de ícones |
| **Toast/Toast** | Sonner | 2.0 | Notificações |
| **UI Framework** | PrimeReact | 10.9 | Componentes prontos |
| **WebSocket** | STOMP.js | 7.1 | Comunicação realtime |
| **Testing** | Vitest | - | Test runner zero-config |
| **Testing Lib** | React Testing Library | 16.3 | Teste de componentes |
| **CSS Utils** | TailwindCSS + clsx | - | Merge de classes CSS |

### Infraestrutura
| Serviço | Versão | Porta | Uso |
|---------|--------|-------|-----|
| **Docker** | 4.0+ | - | Containerização |
| **PostgreSQL** | 15 | 5432 | Banco de dados |
| **MinIO** | latest | 9000/9001 | Object Storage (S3) |
| **Nginx** | Alpine | 80 | Reverse Proxy |
| **Docker Compose** | 3.8 | - | Orquestração |

---

## 🔄 Fluxo de Dados

### 1️⃣ Fluxo de Autenticação

```
┌──────────────────────────────────────────────────────────┐
│                    USUÁRIO ACESSA SITE                    │
└────────────────────┬─────────────────────────────────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │ AuthContext verifica  │
         │ token em localStorage │
         └────────┬──────────────┘
                  │
         ┌────────▼─────────────┐
         │ Token válido?        │
         └────────┬──────┬──────┘
                  │      │
            Sim   │      │   Não
         ┌────────▼┐    ┌▼──────────┐
         │  Home   │    │ Auth Modal│
         │Autenticado   │  Login    │
         └────────┬┐    └┬──────────┘
                  ││     │
                  ││     ▼
                  ││   POST /auth/login
                  ││   (email + senha)
                  ││     │
                  ││     ▼
                  ││   Backend valida
                  ││   gera JWT Token
                  ││     │
                  ││     ▼
                  ││   localStorage.token
                  ││   AuthContext.setToken
                  ││     │
                  │└─────┘
                  │
         ┌────────▼─────────┐
         │ Requisições HTTP │
         │ Header: JWT      │
         └──────────────────┘
```

### 2️⃣ Fluxo de CRUD (Exemplo: Álbuns)

```
┌─────────────────────────────────────┐
│    Componente ReactComponent        │
│  (AlbumsPage.tsx ou Modal)          │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│   albumService.ts (Service Layer)   │
│  ▪ albumService.getAll()            │
│  ▪ albumService.create(data)        │
│  ▪ albumService.update(id, data)    │
│  ▪ albumService.delete(id)          │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│       apiClient.ts (HTTP Client)    │
│  Axios + interceptors + auth header │
└────────────┬────────────────────────┘
             │ HTTP Request
             ▼
    ┌─────────────────────┐
    │  Backend REST API   │
    │  :3333/api/albums   │
    └────────┬────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│     AlbumController (Handler)       │
│  @GetMapping, @PostMapping, etc     │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│    AlbumService (Business Logic)    │
│  - Validações de negócio            │
│  - Tratamento de erros              │
└────────────┬────────────────────────┘
             │
             ▼
┌─────────────────────────────────────┐
│  AlbumRepository (Data Access)      │
│  - Query ao banco com JPA           │
│  - Operações CRUD                   │
└────────────┬────────────────────────┘
             │
             ▼
┌───────────────────────────────┐
│  PostgreSQL Database          │
│  └─ Table: albums            │
│  └─ Table: artists           │
│  └─ Table: album_artists     │
└───────────────────────────────┘
```

### 3️⃣ Fluxo de Upload de Arquivo (Capa de Álbum)

```
┌──────────────────────────────────┐
│  ImageUploadZone.tsx (Frontend)  │
│  Usuário seleciona arquivo       │
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│  albumService.uploadCover()      │
│  FormData com arquivo            │
└────────────┬─────────────────────┘
             │ multipart/form-data
             ▼
┌──────────────────────────────────┐
│  Backend Controller               │
│  /api/albums/{id}/upload-cover   │
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│  MinIOService (Storage Service)  │
│  - putObject()                   │
│  - URL pré-assinada              │
└────────────┬─────────────────────┘
             │
             ▼
┌──────────────────────────────────┐
│  MinIO Server (S3 Compatible)    │
│  └─ Bucket: capas-albuns-media  │
│  └─ /album-123/cover.jpg        │
└──────────────────────────────────┘
```

### 4️⃣ Fluxo de WebSocket (Notificações Realtime)

```
┌────────────────────────────────────┐
│  Frontend conecta ao WebSocket     │
│  (ao fazer login)                  │
└────────────┬───────────────────────┘
             │
             ▼ STOMP/WebSocket
┌────────────────────────────────────┐
│  Backend WebSocket Handler         │
│  /ws/notifications                 │
└────────────┬───────────────────────┘
             │
             ▼
┌────────────────────────────────────┐
│  NotificationCenter (Backend)      │
│  - Gerencia conexões ativas        │
│  - Envia broadcast de eventos      │
└────────────┬───────────────────────┘
             │
   ┌─────────┼─────────┐
   │         │         │
Album criado  │ Usuário adicionado
   │         │         │
   ▼         ▼         ▼
Notifica todos clientes conectados
   │
   ▼
┌────────────────────────────────────┐
│  NotificationCenter (Frontend)     │
│  - Recebe mensagem WebSocket       │
│  - Toast notification aparece      │
│  - UI atualiza em tempo real       │
└────────────────────────────────────┘
```

---

## 🗂️ Padrões de Organização

### Backend - Padrão em Camadas

```
Controller Layer
    ↓ (DTO + Validação)
Service Layer (Business Logic)
    ↓ (Entity)
Repository Layer (DAO)
    ↓
Database (PostgreSQL)
```

**Exemplo - Criar um Álbum:**
1. `POST /api/albums` → AlbumController.create()
2. Valida DTO com @Valid
3. AlbumService.create() → lógica de negócio
4. AlbumRepository.save() → persiste no banco
5. Retorna AlbumResponse.dto

### Frontend - Padrão Modular

```
Pages (Route handlers)
    ↓ (renderiza)
Components (UI elements)
    ↓ (chama)
Services (API calls)
    ↓ (requisita)
API Client (HTTP communication)
```

**Exemplo - Listar Álbuns:**
1. `AlbumsPage.tsx` → monta layout
2. `<AlbumsDataTable />` → renderiza tabela
3. `albumService.getAll()` → busca dados
4. `apiClient.get('/albums')` → HTTP GET
5. Atualiza estado e re-renderiza

---

## 🚀 Como Contribuir

### Adicionando uma Nova Feature no Backend

#### 1. Criar Entity (Modelo de Dados)
```java
// src/main/java/br/gov/mt/seplag/sgd/entity/MyEntity.java
@Entity
@Table(name = "my_table")
@Data
@NoArgsConstructor
public class MyEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
}
```

#### 2. Criar Repository
```java
// src/main/java/br/gov/mt/seplag/sgd/repository/MyEntityRepository.java
public interface MyEntityRepository extends JpaRepository<MyEntity, Long> {
    List<MyEntity> findByNameContaining(String name);
}
```

#### 3. Criar Service
```java
// src/main/java/br/gov/mt/seplag/sgd/service/MyEntityService.java
@Service
@RequiredArgsConstructor
public class MyEntityService {
    private final MyEntityRepository repository;
    
    public List<MyEntity> getAll() {
        return repository.findAll();
    }
}
```

#### 4. Criar Controller
```java
// src/main/java/br/gov/mt/seplag/sgd/controller/MyEntityController.java
@RestController
@RequestMapping("/api/my-entities")
@RequiredArgsConstructor
public class MyEntityController {
    private final MyEntityService service;
    
    @GetMapping
    public ResponseEntity<List<MyEntity>> getAll() {
        return ResponseEntity.ok(service.getAll());
    }
}
```

#### 5. Criar Migration SQL
```sql
-- src/main/resources/db/migration/V16__create_my_table.sql
CREATE TABLE my_table (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
```

### Adicionando uma Nova Feature no Frontend

#### 1. Criar Service
```typescript
// src/services/myEntityService.ts
import { apiClient } from './apiClient';

export const myEntityService = {
  getAll: async () => {
    const response = await apiClient.get('/my-entities');
    return response.data;
  },
  create: async (data) => {
    const response = await apiClient.post('/my-entities', data);
    return response.data;
  },
};
```

#### 2. Criar Tipos TypeScript
```typescript
// src/types/models.ts
export interface MyEntity {
  id: number;
  name: string;
  createdAt: string;
}
```

#### 3. Criar Componente
```typescript
// src/components/MyEntityTable/MyEntityTable.tsx
import { useEffect, useState } from 'react';
import { myEntityService } from '@/services/myEntityService';

export function MyEntityTable() {
  const [entities, setEntities] = useState<MyEntity[]>([]);
  
  useEffect(() => {
    myEntityService.getAll().then(setEntities);
  }, []);
  
  return (
    <div>
      {entities.map(entity => (
        <div key={entity.id}>{entity.name}</div>
      ))}
    </div>
  );
}
```

#### 4. Criar Página
```typescript
// src/pages/my-entities/MyEntitiesPage.tsx
import { MyEntityTable } from '@/components/MyEntityTable';

export function MyEntitiesPage() {
  return (
    <div className="p-6">
      <h1>Minhas Entidades</h1>
      <MyEntityTable />
    </div>
  );
}
```

#### 5. Registrar Rota
```typescript
// src/routes/menuRoutes.ts
export const menuRoutes = [
  // ...
  {
    path: '/my-entities',
    label: 'Minha Entidade',
    icon: 'AiOutlineFile',
  },
  // ...
];
```

---

## 📊 Dependências Principais

### Backend - `pom.xml`
```xml
<!-- Spring Boot 3.4.1 -->
<!-- PostgreSQL Driver -->
<!-- Spring Security + JWT -->
<!-- MinIO Client -->
<!-- Bucket4j (Rate Limiting) -->
<!-- Flyway (Database Migrations) -->
<!-- SpringDoc OpenAPI (Swagger) -->
<!-- Lombok (Reduce boilerplate) -->
```

### Frontend - `package.json`
```json
{
  "dependencies": {
    "react": "^19.2.0",
    "typescript": "^5.9",
    "vite": "^7.2",
    "tailwindcss": "^4.1",
    "axios": "^1.13.2",
    "react-hook-form": "^7.71",
    "zod": "^4.3.5",
    "@stomp/stompjs": "^7.1.2"
  }
}
```

---

## 🐳 Infraestrutura

### Docker Compose Services

| Serviço | Imagem | Porta | Função |
|---------|--------|-------|--------|
| **database** | postgres:15-alpine | 5432 | PostgreSQL DB |
| **backend** | ./backend (Dockerfile) | 3333 | Spring Boot API |
| **storage** | minio/minio:latest | 9000/9001 | Object Storage |
| **storage-setup** | minio/mc | - | Config bucket MinIO |
| **frontend** | ./frontend (Dockerfile) | 5173 | React App + Nginx |

### Variáveis de Ambiente Importante

```bash
# Backend
SPRING_DATASOURCE_URL=jdbc:postgresql://database:5432/db_discografia_core
SPRING_DATASOURCE_USERNAME=srv_discografia
SPRING_DATASOURCE_PASSWORD=P@ssw0rd_Seplag2026!
APP_MINIO_URL=http://storage:9000
APP_MINIO_ACCESS-KEY=admin_storage
APP_MINIO_SECRET-KEY=Storage_Key_2026!

# Frontend
VITE_API_BASE_URL=http://localhost:3333
VITE_WS_BASE_URL=ws://localhost:3333
```

---

## 🔐 Segurança

### Criptografia e Autenticação
- **JWT Tokens:** Validados em cada requisição
- **Password Hashing:** BCrypt (Spring Security)
- **Rate Limiting:** Bucket4j - máx 100 req/min por IP
- **CORS:** Habilitado para frontend
- **HTTPS:** Recomendado em produção

### Boas Práticas
✅ Senhas nunca são retornadas na API
✅ Tokens JWT com expiração
✅ Refresh tokens para renovação
✅ HTTPS em produção
✅ SQL Injection prevenido com JPA/Parameterized queries

---

## 📝 Convenções de Código

### Backend (Java)
- **Classes:** PascalCase (ex: `AlbumService`)
- **Métodos:** camelCase (ex: `getAlbumById`)
- **Constantes:** UPPER_SNAKE_CASE (ex: `MAX_FILE_SIZE`)
- **Pacotes:** lowercase com ponto (ex: `br.gov.mt.seplag.sgd.service`)

### Frontend (TypeScript)
- **Componentes:** PascalCase (ex: `AlbumsDataTable`)
- **Funções:** camelCase (ex: `fetchAlbums`)
- **Interfaces:** PascalCase prefixado com I (ex: `IAlbum`) ou sem I
- **Tipos:** PascalCase (ex: `type AlbumDTO`)
- **Constantes:** UPPER_SNAKE_CASE (ex: `API_BASE_URL`)

---

## 📚 Referências Úteis

- [Spring Boot Docs](https://spring.io/projects/spring-boot)
- [React Docs](https://react.dev)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Vite Docs](https://vitejs.dev/)
- [Docker Docs](https://docs.docker.com/)

---

**Última atualização:** Fevereiro 2026
**Versão do Projeto:** 0.0.1-SNAPSHOT
