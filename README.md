# API de Gestão de Inventário

![Tests](https://github.com/guto-henrique/inventory-api/actions/workflows/tests.yml/badge.svg)

API REST para gerenciamento de categorias, produtos e movimentações de estoque, com regras de negócio, migrations versionadas e testes automatizados.

## Tecnologias

Python, FastAPI, Pydantic, SQLAlchemy, PostgreSQL, Alembic, Pytest e Docker.

## Executar

**Pré-requisitos:** Docker, Docker Compose e Python 3.11+.

```bash
git clone https://github.com/guto-henrique/inventory-api.git
cd inventory-api

cp .env.example .env

docker compose up --build -d
docker compose exec api alembic upgrade head
```

A API estará disponível em `http://localhost:8000`.

Para carregar dados de exemplo:

```bash
docker compose exec api python -m app.seed
```

Para parar a aplicação:

```bash
docker compose down
```

## Documentação

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

![alt text](image.png)

## Endpoints

| Recurso | Operações |
| --- | --- |
| Categorias | `POST`, `GET`, `GET/{id}`, `PUT/{id}`, `DELETE/{id}` |
| Produtos | `POST`, `GET`, `GET/{id}`, `PUT/{id}`, `DELETE/{id}` |
| Movimentações | `POST`, `GET`, `GET/{id}`, `DELETE/{id}` |

## Regras de negócio

- O estoque não pode ficar negativo.
- Produtos inativos não recebem movimentações.
- Categorias com produtos não podem ser excluídas.
- Produtos usam soft delete.
- Movimentações não podem ser editadas.
- A exclusão de uma movimentação reverte o estoque.

## Testes

Defina `TEST_DATABASE_URL` no `.env` (veja `.env.example`) e execute:

```bash
pytest -v
```

Os testes também rodam automaticamente no GitHub Actions a cada push.

## Estrutura

```
alembic/     migrations do banco
app/         código da aplicação (models, schemas, routers)
tests/       testes automatizados
```