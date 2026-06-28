# 🏭 Agent Factory Core

> Plataforma profesional para crear, validar, evaluar y exportar agentes IA multi-LLM.
> Integrable con Paquete y Navaja Suiza.

[![Tests](https://github.com/BGWebDevelopment/agent-factory-core/actions/workflows/test.yml/badge.svg)](https://github.com/BGWebDevelopment/agent-factory-core/actions/workflows/test.yml)
[![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/downloads/release/python-3110/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.111-green.svg)](https://fastapi.tiangolo.com/)

---

## ¿Qué es Agent Factory Core?

**Agent Factory Core** no es un constructor visual de agentes. Es una **fábrica profesional**:

- ✅ Los agentes se definen **declarativamente en YAML**
- ✅ Se **evalúan automáticamente** con promptfoo antes de deployar
- ✅ Soportan **múltiples LLMs** (OpenAI, Anthropic, Ollama, Groq...)
- ✅ Tienen **skills modulares** inyectadas por defecto
- ✅ Se **exportan** a Paquete, Navaja Suiza, Docker, MCP, JSON

---

## Propuesta de Valor

| Métrica | Impacto |
|---------|---------|
| Time to market | **-80%** (días → horas) |
| Bugs en producción | **-90%** (testing automático) |
| Vendor lock-in | **0** (multi-LLM nativo) |
| Seguridad | **Por defecto** (no afterthought) |

---

## Quick Start

```bash
# 1. Clonar el repo
git clone https://github.com/BGWebDevelopment/agent-factory-core.git
cd agent-factory-core

# 2. Configurar variables de entorno
cp .env.example .env
# Edita .env con tus API keys

# 3. Arrancar con Docker
docker-compose up -d

# 4. Verificar que funciona
curl http://localhost:8000/health
# {"status": "ok", "version": "0.1.0"}

# 5. Ver la API docs
open http://localhost:8000/docs
```

---

## Tu primer agente en 5 minutos

```yaml
# mi_agente.yaml
core:
  agent_id: mi_primer_agente
  name: "Mi Primer Agente"
  purpose: "Responde preguntas sobre el proyecto"
  agent_version: "1.0.0"
  default_llm: "openai:gpt-4o"
  mandatory_skills:
    - prompt_injection_defense
    - tool_safety
    - cost_guard
```

```bash
# Crear el agente via API
curl -X POST http://localhost:8000/api/v1/agents \
  -H "Content-Type: application/json" \
  -d @mi_agente.yaml

# Lanzar evaluación
curl -X POST http://localhost:8000/api/v1/agents/mi_primer_agente/evaluate

# Exportar a Docker
curl http://localhost:8000/api/v1/agents/mi_primer_agente/export?format=docker
```

---

## Arquitectura

```
Frontend (React + TypeScript)  →  Dashboard, Wizard de agentes
Backend (FastAPI + Python)     →  API, lógica de negocio
Jobs (Celery + Redis)          →  Evaluaciones asíncronas
Storage (PostgreSQL + Redis)   →  Datos, caché
Evals (promptfoo CLI)          →  Testing automático
LLM Router (LiteLLM)           →  Multi-LLM sin vendor lock-in
```

---

## Skills Base Incluidas

| Skill | Descripción | Obligatoria |
|-------|-------------|-------------|
| `prompt_injection_defense` | Protege contra ataques de inyección | ✅ Sí |
| `tool_safety` | Whitelist de herramientas, deny by default | ✅ Sí |
| `cost_guard` | Control de presupuesto por llamada/día | ✅ Sí |
| `prompt_optimization` | Optimiza prompts antes del LLM | Opcional |
| `output_validation` | Valida outputs antes de retornarlos | Opcional |
| `human_approval` | Requiere aprobación para acciones críticas | Opcional |

---

## LLMs Soportados

| Provider | Modelos | Privacidad |
|----------|---------|------------|
| OpenAI | gpt-4o, gpt-4o-mini | 🔴 Baja |
| Anthropic | claude-3-5-sonnet, claude-3-opus | 🔴 Baja |
| Groq | llama-3.1-70b, mixtral-8x7b | 🔴 Baja |
| Ollama | llama3.1:8b, qwen2.5:7b (local) | 🟢 Alta |

**Estrategias de routing**: `privacy_first` | `cost_optimized` | `quality_first` | `latency_first`

---

## Stack Tecnológico

| Capa | Tecnología |
|------|-----------|
| Backend | FastAPI + Python 3.11 |
| Frontend | React 18 + TypeScript + Vite |
| Database | PostgreSQL 15 |
| Cache/Queue | Redis 7 |
| Jobs | Celery 5 |
| LLM Router | LiteLLM |
| Evals | promptfoo CLI |
| Container | Docker + docker-compose |
| CI/CD | GitHub Actions |

---

## Exportadores

- 📦 **JSON/YAML**: Configuración portátil del agente
- 🐳 **Docker**: Agente como servicio containerizado
- 🧠 **Paquete**: Integración con tu segundo cerebro
- 🔧 **Navaja Suiza**: Integración con herramienta operativa
- 🔌 **MCP**: Protocolo abierto de Anthropic *(Fase 4)*

---

## Documentación

- [CLAUDE.md](CLAUDE.md) — Contexto para Claude Code (leer esto primero)
- [HANDOFF.md](HANDOFF.md) — Contexto completo del proyecto
- [docs/schema.v1.yaml](docs/schema.v1.yaml) — Schema oficial del agente
- [docs/architecture.md](docs/architecture.md) — Arquitectura detallada
- [CONTRIBUTING.md](CONTRIBUTING.md) — Guía de contribución

---

## Roadmap (9 semanas)

- **Semana 1-3**: Foundation (FastAPI + Agent Model + Skills)
- **Semana 4-6**: Evaluación (promptfoo + Multi-LLM Router + Exporters)
- **Semana 7-9**: Producción (Security + Frontend + Launch)

---

## Licencia

Privado — BGWebDevelopment © 2026
