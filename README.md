# GeoInfo — CRUD de Continentes, Países e Cidades

**GeoInfo** é uma aplicação web completa (frontend + backend + banco) para **cadastrar, consultar, editar e excluir** informações de continentes, países e cidades, com dados complementares de APIs externas:

- **REST Countries**: idioma, moeda, população, área, fuso horário, bandeira.  
- **OpenWeatherMap**: clima atual por latitude/longitude.  
- **Leaflet/OpenStreetMap**: mapas interativos; bandeiras via REST Countries/FlagCDN.

Tudo roda com **Docker Compose**: PostgreSQL + Backend (Express/Prisma) + Frontend (React/Vite/Nginx).

---

## Estrutura

```
crud-ts-pweb/
├── backend/   # submódulo Git
├── frontend/  # submódulo Git
├── docker-compose.yml
└── README.md
```

- Frontend: `http://localhost:8080`  
- Backend: `http://localhost:3333` (API em `/api`)  
- PostgreSQL: porta `5432`

---

## Requisitos

- Git  
- Docker + Docker Compose  

> Node/npm não é necessário em Docker; apenas para desenvolvimento local.

---

## Rodando do zero

### 1) Clonar repositório

```bash
git clone --recurse-submodules https://github.com/m-germano/crud-ts-pweb.git
cd crud-ts-pweb
```

Se já clonou sem submodules:

```bash
git submodule update --init --recursive
```

### 2) Variáveis de ambiente (opcional)

Para OpenWeatherMap, crie `.env` **na raiz do backend**:

```
OPENWEATHER_API_KEY=coloque_sua_api_key_aqui
```

### 3) Subir containers

⚠️ **Cuidado**: `--remove-orphans` remove containers não gerenciados pelo compose.

```bash
docker compose down --volumes --remove-orphans  # limpa containers e volumes antigos
docker compose build --no-cache                 # build do zero
docker compose up -d                             # sobe em background
```

### 4) Verificar

- Frontend: `http://localhost:8080`  
- API: `http://localhost:3333/api/continents`  
- Health: `http://localhost:3333/health`  
- Swagger: `http://localhost:3333/docs`

---

## Comandos úteis

- **Atualizar submódulos**:

```bash
git submodule update --remote --merge
git add frontend backend
git commit -m "Atualiza submódulos"
git push
```

- **Reset completo** (apaga dados!):

```bash
docker compose down --volumes --remove-orphans
docker builder prune -af
docker compose build --no-cache
docker compose up -d
```

- **Ver logs**:

```bash
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
```

---

## Fluxo funcional resumido

- CRUD: Continentes ↔ Países ↔ Cidades  
- Integrações externas: REST Countries, OpenWeatherMap  
- Interface: React + TypeScript + Tailwind/shadcn  
- Navegação: sidebar, listas com filtros/paginação, formulários de cadastro/edição, painel com mapa e bandeiras

---

## 🎥 Vídeo

Vídeo mostrando **como rodar o projeto do zero** usando o README.md como referência:  


https://github.com/user-attachments/assets/635fb137-a697-41ef-bcb0-1c61b6694d31



---

## Licença

Projeto acadêmico/educacional. Ajuste conforme necessário.

