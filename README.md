# GeoInfo — CRUD de Continentes, Países e Cidades (com APIs externas)

**GeoInfo** é uma aplicação web completa (frontend + backend + banco) para **cadastrar, consultar, editar e excluir** informações de **continentes, países e cidades**.  
Cada **cidade** pertence a um **país** e cada **país** pertence a um **continente**.  
Além do CRUD, a aplicação **consome APIs externas** para complementar os dados e enriquecer a interface:

- **REST Countries**: idioma, moeda, população estimada, área, fuso horário, bandeira etc.  
- **OpenWeatherMap**: clima atual por latitude/longitude da cidade.  
- (UI) **Leaflet/OpenStreetMap** e **FlagCDN/REST Countries** para mapa e bandeiras.

Tudo roda com **Docker Compose**: **PostgreSQL + Backend (Express/Prisma) + Frontend (React/Vite/Nginx)**.

---

## 📁 Estrutura do repositório

Este repositório usa **submódulos Git**:

```
crud-ts-pweb/
├── backend/   # submódulo: https://github.com/m-germano/backend-pweb-crud
├── frontend/  # submódulo: https://github.com/m-germano/frontend-pweb-crud
├── docker-compose.yml
└── README.md
```

> O frontend publica em **http://localhost:8080**.  
> O backend publica em **http://localhost:3333** (API em `/api`).  
> O banco PostgreSQL expõe **5432**.

---

## ✅ Requisitos

- **Git** (para clonar com submodules)
- **Docker** e **Docker Compose** (Docker Desktop no Windows/Mac; `docker compose` no Linux)

> Não é necessário Node/npm para rodar em Docker (apenas para desenvolvimento local opcional).

---

## 🚀 Como rodar (limpo, do zero)

> Os comandos abaixo funcionam no **Linux/macOS (bash)** e **Windows PowerShell** (ajuste `\` vs `/` se necessário).

### 1) Clone com submódulos

```bash
git clone --recurse-submodules https://github.com/m-germano/crud-ts-pweb.git
cd crud-ts-pweb
```

Se você já clonou **sem** submodules, inicialize-os:

```bash
git submodule update --init --recursive
```

> Para atualizar os submódulos para as últimas versões dos repos de front/back:
>
> ```bash
> git submodule update --remote --merge
> git add frontend backend
> git commit -m "Atualiza submódulos frontend e backend"
> ```

### 2) (Opcional) Configurar variáveis de ambiente

O `docker-compose.yml` já define credenciais do Postgres e **injeta** `DATABASE_URL` para o backend. 

Se quiser **clima via OpenWeatherMap**, crie um arquivo `.env` **na raiz do backend** (mesmo nível do `docker-compose.yml`) com:

```
OPENWEATHER_API_KEY=coloque_sua_api_key_aqui
```

> Não é necessário definir `VITE_API_BASE_URL` no frontend em Docker — o Nginx do container faz proxy de **/api** para o backend.

### 3) Subir tudo do zero (build sem cache)

> Primeiro, garanta que não há nada antigo rodando:

```bash
docker compose down --volumes --remove-orphans
```

> Agora, **build do zero** e subir:

```bash
docker compose build --no-cache
docker compose up -d
```

Aguarde o banco ficar **healthy**, o backend aplicar **migrations** e executar o **seed** (idempotente), e o frontend publicar a SPA.

### 4) Verificar

- Frontend: **http://localhost:8080**
- API (direto): **http://localhost:3333/api/continents**
- Health: **http://localhost:3333/health**
- Swagger: **http://localhost:3333/docs**

---

## 🧭 Fluxo funcional (resumo)

- **CRUD**:
  - Continentes ↔ Países ↔ Cidades (relacionamentos garantidos via Prisma).
- **Integrações**:
  - **REST Countries**: auto-preenche dados do país (idioma, moeda, fuso, população, área, ISO/bandeira).
  - **OpenWeatherMap**: mostra **clima** da cidade (via lat/lon).
  - **Leaflet / OpenStreetMap**: **mapas** interativos na UI.
  - **Flags**: exibição de bandeiras (via REST Countries/FlagCDN).
- **Interface**:
  - React + TypeScript, React Router, Tailwind + shadcn (estilo básico).
  - Sidebar com navegação; páginas de lista com filtros e paginação; formulários de cadastro/edição; painel com dados externos (bandeira, clima, mapa).

---

## 🔧 Comandos úteis

### Atualizar submódulos (front/back) para o último commit remoto

```bash
git submodule update --remote --merge
git add frontend backend
git commit -m "Atualiza submódulos para últimas versões"
git push
```

### Resetar completamente (containers, volumes, imagens de cache)

⚠️ Cuidado: apaga volume do Postgres e dados!

```bash
docker compose down --volumes --remove-orphans
docker builder prune -af
docker compose build --no-cache
docker compose up -d
```

### Ver logs

```bash
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
```

---


## 📄 Licença

Projeto acadêmico/educacional. Ajuste a licença conforme sua necessidade.
