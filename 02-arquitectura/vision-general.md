# Visión General de Arquitectura

Arquitectura **DDD + Hexagonal** sobre capas limpias monolíticas.

## Capas
1. **Dominio**: Entidades, Value Objects, Servicios de dominio.
2. **Aplicación**: Casos de uso, orquestación.
3. **Infraestructura**: Repositorios (SQLAlchemy), Adaptadores externos.
4. **Presentación**: API REST (FastAPI).

## Stack
- **Backend**: Python 3.12, FastAPI, SQLAlchemy, Alembic.
- **Frontend**: React 18, Vite, Tailwind CSS.
- **BD**: PostgreSQL 15.
- **DevOps**: Docker Compose, Nginx.
