# CLAUDE.md — Agent Factory Core

> Este fichero es leído automáticamente por Claude Code al arrancar.
> Contiene TODO el contexto necesario para trabajar en este proyecto.

---

## ¿QUÉ ES ESTE PROYECTO?

**Agent Factory Core** es una plataforma que permite crear, validar, evaluar y exportar agentes IA multi-LLM de forma profesional y reproducible.

No es un constructor visual de agentes. Es una **fábrica profesional**:
- Los agentes se definen declarativamente en YAML
- Se evalúan automáticamente con promptfoo antes de deployar
- Soportan múltiples LLMs (OpenAI, Anthropic, Ollama, Groq, etc.)
- Tienen skills modulares inyectadas por defecto
- Se exportan a Paquete, Navaja Suiza, Docker, MCP, JSON

---

## DUEÑO Y CONTEXTO

- **Proyecto de**: Borja Gutiérrez (BGWebDevelopment)
- **Integraciones target**: Paquete (segundo cerebro), Navaja Suiza (herramienta operativa)
- **Repositorio**: BGWebDevelopment/agent-factory-core
- **Estado actual**: Fase 1, Sprint 1.1 — Setup inicial

---

## STACK TECNOLÓGICO

| Capa | Tecnología |
|------|-----------|
| Backend | FastAPI + Python 3.11 |
| ORM | SQLAlchemy 2.x + Alembic |
| Frontend | React 18 + TypeScript + Vite |
| DB | PostgreSQL 15 |
| Cache/Queue | Redis 7 |
| Jobs | Celery 5 |
| LLM Router | LiteLLM |
| Evals | promptfoo CLI |
| Container | Docker + docker-compose |
| CI/CD | GitHub Actions |
| Logs | structlog + Prometheus |
| Auth | JWT (python-jose) |

---

## ESTRUCTURA DE CARPETAS

```
agent-factory-core/
├── CLAUDE.md                    ← ESTE FICHERO (leído automáticamente por Claude Code)
├── HANDOFF.md                   ← Contexto completo del proyecto
├── README.md
├── ARCHITECTURE.md
├── CONTRIBUTING.md
├── docker-compose.yml
├── .env.example
├── .gitignore
├── backend/
│   ├── app/
│   │   ├── main.py              ← FastAPI entry point
│   │   ├── config.py            ← Settings via pydantic-settings
│   │   ├── auth.py              ← JWT + RBAC
│   │   ├── database.py          ← SQLAlchemy engine + session
│   │   ├── api/
│   │   │   ├── agents.py
│   │   │   ├── skills.py
│   │   │   ├── evals.py
│   │   │   └── exports.py
│   │   ├── models/
│   │   │   ├── agent.py
│   │   │   ├── skill.py
│   │   │   └── eval_result.py
│   │   ├── schemas/
│   │   │   ├── agent.py
│   │   │   └── skill.py
│   │   ├── services/
│   │   │   ├── agent_registry.py
│   │   │   ├── skill_registry.py
│   │   │   ├── llm_router.py
│   │   │   ├── eval_runner.py
│   │   │   └── exporter.py
│   │   ├── security/
│   │   │   ├── pii_detector.py
│   │   │   └── rate_limiter.py
│   │   └── utils/
│   │       ├── logger.py
│   │       └── validators.py
│   ├── skills/
│   │   ├── base.py
│   │   ├── prompt_optimization.py
│   │   ├── prompt_injection_defense.py
│   │   ├── tool_safety.py
│   │   ├── cost_guard.py
│   │   └── eval_generation.py
│   ├── exporters/
│   │   ├── base.py
│   │   ├── json_exporter.py
│   │   ├── yaml_exporter.py
│   │   ├── docker_exporter.py
│   │   ├── paquete_exporter.py
│   │   └── navaja_exporter.py
│   ├── jobs/
│   │   ├── celery_app.py
│   │   ├── eval_tasks.py
│   │   └── export_tasks.py
│   ├── migrations/versions/
│   ├── tests/
│   │   ├── conftest.py
│   │   ├── test_agents.py
│   │   ├── test_skills.py
│   │   └── test_exporters.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── alembic.ini
├── frontend/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   ├── api/client.ts
│   │   ├── components/
│   │   │   ├── AgentWizard/
│   │   │   │   ├── index.tsx
│   │   │   │   ├── Step1_Metadata.tsx
│   │   │   │   ├── Step2_Purpose.tsx
│   │   │   │   ├── Step3_LLMs.tsx
│   │   │   │   ├── Step4_Skills.tsx
│   │   │   │   └── Preview.tsx
│   │   │   ├── AgentEditor.tsx
│   │   │   ├── SkillManager.tsx
│   │   │   └── Dashboard.tsx
│   │   ├── pages/
│   │   │   ├── Home.tsx
│   │   │   ├── AgentDetail.tsx
│   │   │   ├── EvalResults.tsx
│   │   │   └── Login.tsx
│   │   ├── stores/agentStore.ts
│   │   └── types/agent.ts
│   ├── package.json
│   ├── tsconfig.json
│   ├── vite.config.ts
│   └── Dockerfile
├── templates/
│   ├── agents/
│   │   ├── customer_support.yaml
│   │   ├── code_reviewer.yaml
│   │   └── research_analyst.yaml
│   └── promptfoo/base_test_suite.yaml
├── docs/
│   ├── index.md
│   ├── architecture.md
│   ├── schema.v1.yaml
│   ├── api.md
│   ├── security-threat-model.md
│   └── deployment.md
└── .github/
    ├── workflows/
    │   ├── test.yml
    │   ├── security.yml
    │   └── deploy-staging.yml
    ├── ISSUE_TEMPLATE/
    │   ├── bug.md
    │   ├── feature.md
    │   └── security.md
    └── pull_request_template.md
```

---

## REGLAS DE DESARROLLO (SEGUIR SIEMPRE)

### Código
1. **Python**: Tipado estricto con mypy. Nunca `Any` sin justificación
2. **Async**: Todas las rutas FastAPI y servicios son `async def`
3. **Validación**: Siempre Pydantic schemas. Nunca dicts sin validar
4. **Logging**: Usar structlog con campos contextuales, nunca `print()`
5. **Tests**: Cada función nueva = test correspondiente. Target: 80%+
6. **Migraciones**: Cada cambio de schema = nueva migración Alembic

### Git
1. **Branches**: `feature/`, `fix/`, `docs/`, `chore/`
2. **Commits**: Conventional Commits (`feat:`, `fix:`, `docs:`, `test:`)
3. **PRs**: Siempre con tests, nunca merge sin CI verde

### Seguridad
1. **Secrets**: Solo via variables de entorno, nunca hardcodeados
2. **PII**: Detectar y enmascarar antes de enviar a LLM externo
3. **Tools**: Lista blanca explícita, deny_by_default
4. **Inputs**: Validar y sanitizar todo antes de procesar

---

## VARIABLES DE ENTORNO REQUERIDAS

```bash
DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/agentfactory
REDIS_URL=redis://localhost:6379/0
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GROQ_API_KEY=gsk_...
SECRET_KEY=generate-with-openssl-rand-hex-32
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440
ENVIRONMENT=development
LOG_LEVEL=INFO
ALLOWED_ORIGINS=http://localhost:3000
```

---

## ROADMAP ACTIVO (9 Semanas)

### FASE 1: Foundation (Semanas 1-3) ← EN CURSO
- [ ] Sprint 1.1: Repo setup + FastAPI boilerplate + observabilidad base
- [ ] Sprint 1.2: Agent model + CRUD API + schema YAML v1
- [ ] Sprint 1.3: Skill system + 3 skills base (injection_defense, tool_safety, cost_guard)

### FASE 2: Evaluación (Semanas 4-6)
- [ ] Sprint 2.1: promptfoo integration + auto-test generation
- [ ] Sprint 2.2: Multi-LLM router con RoutingStrategy
- [ ] Sprint 2.3: Exportadores (JSON, YAML, Docker, Paquete)

### FASE 3: Producción (Semanas 7-9)
- [ ] Sprint 3.1: Security engine + PII detection
- [ ] Sprint 3.2: Frontend React + Dashboard
- [ ] Sprint 3.3: Docs completas + launch

---

## DECISIONES ARQUITECTÓNICAS CLAVE

| Decisión | Elegido | Razón |
|----------|---------|-------|
| Backend | FastAPI | Async nativo, auto-docs, tipado |
| LLM abstraction | LiteLLM | Multi-provider sin boilerplate |
| Job queue | Celery | Escala, retry, monitoring |
| Eval framework | promptfoo | Industry standard |
| MCP position | Adaptador externo (Fase 4) | Riesgos seguridad STDIO |
| Agent storage | PostgreSQL JSONB | ACID + flexibilidad JSON |

---

## COMANDOS ÚTILES

```bash
# Setup
bash scripts/setup_local.sh

# Backend dev
cd backend && uvicorn app.main:app --reload --port 8000

# Frontend dev
cd frontend && npm run dev

# Tests
cd backend && pytest tests/ -v --cov=app --cov-report=html

# Migrations
cd backend && alembic upgrade head

# Docker completo
docker-compose up -d

# Celery worker
cd backend && celery -A jobs.celery_app worker --loglevel=info
```

---

## INTEGRACIONES TARGET

### Paquete
- Segundo cerebro del usuario (Borja)
- El exporter genera formato específico de Paquete
- Formato exacto: **PREGUNTAR AL USUARIO antes de implementar**

### Navaja Suiza
- Herramienta operativa multiusos
- Formato exacto: **PREGUNTAR AL USUARIO antes de implementar**

### MCP (Model Context Protocol)
- Protocolo de Anthropic para conexión entre agentes y herramientas
- Implementar en **Fase 4** (post-MVP)
- Usar como adaptador externo, NO como núcleo del sistema

---

## LEE TAMBIÉN
- `HANDOFF.md` — Contexto completo, decisiones tomadas, problemas identificados
- `docs/schema.v1.yaml` — Schema oficial del agente (fuente de verdad)
- `docs/architecture.md` — Diagramas y decisiones arquitectónicas
- `CONTRIBUTING.md` — Guía de contribución y workflow de PRs
