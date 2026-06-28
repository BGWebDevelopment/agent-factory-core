# CLAUDE.md — Agent Factory Core
# Versión 2.0 — Documento de referencia para Claude Code

> ⚡ Este fichero es leído AUTOMÁTICAMENTE por Claude Code al arrancar en este repo.
> Es la fuente de verdad del proyecto. Léelo completo antes de escribir una sola línea.
> Si algo en el código contradice este fichero, el fichero gana.

---

## 1. VISIÓN DEL PRODUCTO

### ¿Qué es Agent Factory Core?

**Agent Factory Core** es una plataforma de infraestructura para crear, optimizar,
evaluar y exportar agentes IA de forma profesional.

**NO es**: un constructor visual de chatbots, un wrapper de OpenAI, ni un framework
que te ata a un proveedor.

**SÍ es**: una fábrica profesional, agnóstica, declarativa y optimizada para producción.

### Los 5 pilares del sistema

```
PILAR 1 — AGENTES DECLARATIVOS
  Los agentes se definen en YAML versionado.
  No prompts sueltos. No código frágil.
  Reproducibles, auditables, testeables.

PILAR 2 — SKILLS (Motor de Capacidades)
  15 módulos reutilizables que se inyectan en cualquier agente.
  Desde optimización de tokens hasta defensa contra inyección.
  Composables, versionados, testeados individualmente.

PILAR 3 — GEMS (Gemas — Conocimiento Reutilizable)
  Fragmentos de contexto/instrucción/conocimiento que se definen
  una vez y cualquier agente puede incluir.
  Ej: gem_tono_empresa, gem_politica_devolucion, gem_glosario_tecnico
  Actualizar una gem = todos los agentes que la usan se actualizan.

PILAR 4 — OPTIMIZATION ENGINE
  Sistema que reduce costes y tiempos automáticamente:
  - Context compression: -40/70% tokens sin perder calidad
  - Response cache: -60% llamadas repetidas (cache semántico)
  - Smart routing: modelo correcto para cada tarea
  - Batch processing: +300% throughput en tareas masivas
  - Prompt optimization: -15/25% tokens por limpieza

PILAR 5 — TOTAL PROVIDER AGNOSTICISM
  Funciona con CUALQUIER proveedor LLM:
  OpenAI · Anthropic · Google · Groq · Mistral · Ollama · OpenRouter
  Azure OpenAI · AWS Bedrock · Cohere · y cualquier OpenAI-compatible
  Cambiar de proveedor = cambiar 1 línea en el YAML del agente.
  El código no cambia. Las skills no cambian. Las gems no cambian.
```

---

## 2. CONTEXTO Y PROPIETARIO

- **Proyecto de**: Borja Gutiérrez (BGWebDevelopment)
- **Repositorio**: BGWebDevelopment/agent-factory-core
- **Integraciones target**:
  - **Paquete**: Segundo cerebro personal del usuario. Los agentes se instalan como módulos.
  - **Navaja Suiza**: Herramienta operativa multiusos. Formato de integración: PREGUNTAR A BORJA.
- **Modelo de despliegue MVP**: local/self-hosted. Multi-tenant es opcional post-MVP.
- **Estado**: Fase 1, Sprint 1.1 — Foundation

---

## 3. ARQUITECTURA COMPLETA DEL SISTEMA

```
╔══════════════════════════════════════════════════════════════════════╗
║                      AGENT FACTORY CORE                            ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐  ║
║  │  FRONTEND (React 18 + TypeScript + Vite)                     │  ║
║  │  Wizard · Editor · Dashboard · Gem Manager · Skill Browser   │  ║
║  └─────────────────────────┬────────────────────────────────────┘  ║
║                             │ REST API / WebSocket                   ║
║  ┌──────────────────────────▼────────────────────────────────────┐  ║
║  │  BACKEND (FastAPI + Python 3.11)                              │  ║
║  │                                                                │  ║
║  │  ┌────────────┐  ┌────────────┐  ┌────────────┐              │  ║
║  │  │   AGENT    │  │   SKILL    │  │    GEM     │              │  ║
║  │  │  REGISTRY  │  │  ENGINE    │  │  REGISTRY  │              │  ║
║  │  │            │  │            │  │            │              │  ║
║  │  │ Definición │  │ 15 skills  │  │ Contexto   │              │  ║
║  │  │ Versioning │  │ Composable │  │ Reutilizable│             │  ║
║  │  │ Políticas  │  │ Testeables │  │ Versionado │              │  ║
║  │  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘             │  ║
║  │        └───────────────┼───────────────┘                      │  ║
║  │                        │                                       │  ║
║  │  ┌─────────────────────▼───────────────────────────────────┐  │  ║
║  │  │              OPTIMIZATION ENGINE                         │  │  ║
║  │  │  Context Compressor · Response Cache · Prompt Optimizer  │  │  ║
║  │  │  Batch Processor · Cost Tracker · Latency Monitor        │  │  ║
║  │  └─────────────────────┬───────────────────────────────────┘  │  ║
║  │                        │                                       │  ║
║  │  ┌─────────────────────▼───────────────────────────────────┐  │  ║
║  │  │              LLM ROUTER (agnóstico)                      │  │  ║
║  │  │  Privacy · Cost · Quality · Speed strategies             │  │  ║
║  │  │  Circuit breaker · Fallback chains · Rate limiting        │  │  ║
║  │  └─────────────────────┬───────────────────────────────────┘  │  ║
║  │                        │                                       │  ║
║  │  ┌─────────────────────▼───────────────────────────────────┐  │  ║
║  │  │              SECURITY ENGINE                             │  │  ║
║  │  │  PII Detection · Tool Sandbox · Audit Logger · RBAC      │  │  ║
║  │  └─────────────────────┬───────────────────────────────────┘  │  ║
║  │                        │                                       │  ║
║  │  ┌─────────────────────▼───────────────────────────────────┐  │  ║
║  │  │              EVAL RUNNER (promptfoo)                     │  │  ║
║  │  │  Auto test generation · Red teaming · Score gate ≥0.85   │  │  ║
║  │  └─────────────────────┬───────────────────────────────────┘  │  ║
║  │                        │                                       │  ║
║  │  ┌─────────────────────▼───────────────────────────────────┐  │  ║
║  │  │              EXPORTERS                                   │  │  ║
║  │  │  Paquete · Navaja Suiza · Docker · JSON · YAML · MCP     │  │  ║
║  │  └─────────────────────────────────────────────────────────┘  │  ║
║  └───────────────────────────────────────────────────────────────┘  ║
║                                                                      ║
║  ┌────────────────────────────────────────────────────────────────┐  ║
║  │  JOBS (Celery + Redis)                                         │  ║
║  │  eval_tasks · export_tasks · optimization_tasks · cache_tasks  │  ║
║  └────────────────────────────────────────────────────────────────┘  ║
║                                                                      ║
║  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌──────────┐  ║
║  │ PostgreSQL  │  │    Redis    │  │   Qdrant    │  │  Files   │  ║
║  │ (principal) │  │ (cache+jobs)│  │ (RAG+cache) │  │ (configs)│  ║
║  └─────────────┘  └─────────────┘  └─────────────┘  └──────────┘  ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 4. LAS 15 SKILLS BASE DEL SISTEMA

Las skills son módulos de capacidad que se inyectan en los agentes.
Cada skill tiene: metadata, validate(), execute(), get_system_prompt_injection().

### Skills de Optimización (reducen coste y tiempo)
```
skill_prompt_optimization       → Limpia y optimiza el prompt antes del LLM (-15/25% tokens)
skill_context_compression       → Comprime historial de conversación (-40/70% tokens)
skill_model_router              → Elige el modelo correcto para cada tarea
skill_response_cache            → Cacha respuestas, evita llamadas duplicadas (-60% llamadas)
skill_skill_recommender         → Sugiere qué skills activar según el uso del agente
```

### Skills de Seguridad (protegen el sistema)
```
skill_prompt_injection_defense  → Bloquea/sanitiza ataques de prompt injection
skill_tool_safety               → Whitelist de herramientas, deny_by_default
skill_output_validation         → Valida respuesta del LLM antes de retornarla
skill_human_approval            → Requiere confirmación humana para acciones críticas
skill_error_recovery            → Manejo de errores, reintentos con backoff
```

### Skills de Governance (control y auditoría)
```
skill_cost_guard                → Presupuesto por llamada/día/mes, alertas, bloqueo
skill_logging_observability     → Logs estructurados, métricas, trazas de cada llamada
skill_eval_generation           → Genera test suite automática para el agente
skill_promptfoo_test_runner     → Ejecuta evaluaciones con promptfoo CLI
skill_memory_policy             → Qué recordar, qué olvidar, qué cifrar, PII handling
```

**REGLA**: Las skills `prompt_injection_defense`, `tool_safety` y `cost_guard`
son **MANDATORY** en todos los agentes. No se pueden desactivar en MVP.

---

## 5. SISTEMA DE GEMAS (GEMS)

Las gemas son fragmentos de contexto, instrucción o conocimiento que se definen
una vez y cualquier agente puede incluir. Resuelven el problema de duplicación
de contexto y facilitan la actualización centralizada.

### Tipos de gemas
```
IDENTITY GEMS     → Tono, voz, personalidad de marca
KNOWLEDGE GEMS    → Información de dominio (catálogo, políticas, glosario)
INSTRUCTION GEMS  → Reglas de comportamiento reutilizables
CONTEXT GEMS      → Contexto situacional (empresa, producto, mercado)
SECURITY GEMS     → Restricciones de seguridad específicas del cliente
```

### Cómo funcionan
```yaml
# Definición de una gema (una vez)
gem_id: gem_tono_empresa_v1
type: identity
content: |
  Eres un asistente de Empresa X. Tu tono es profesional pero cercano.
  Usas siempre el nombre del cliente. Nunca uses jerga técnica innecesaria.
token_count: 42
version: "1.0.0"

# Uso en agente (múltiples agentes)
agent:
  core:
    agent_id: soporte_cliente
    gems:
      - gem_tono_empresa_v1       # ← referencia, no duplicado
      - gem_politica_devolucion_v2
      - gem_catalogo_productos_v1
```

### Impacto en tokens
Sin gems: 3 agentes x 500 tokens de contexto compartido = **1.500 tokens duplicados**
Con gems: 3 referencias a gems = **42 tokens** + 1 vez cargado = **-97% duplicación**

---

## 6. OPTIMIZATION ENGINE — ESPECIFICACIÓN

El Optimization Engine se ejecuta en el pipeline de cada llamada LLM:

```
INPUT (prompt + contexto)
        │
        ▼
[1] PROMPT OPTIMIZER
    → Elimina redundancias
    → Estructura instrucciones
    → Aplica best practices
    → Output: prompt limpio (-15/25% tokens)
        │
        ▼
[2] CONTEXT COMPRESSOR
    → Si contexto > max_tokens * 0.7: comprime
    → Sliding window + summarization semántica
    → Preserva información crítica
    → Output: contexto comprimido (-40/70%)
        │
        ▼
[3] CACHE LOOKUP
    → Hash semántico del prompt
    → Si hit en cache: devuelve sin llamar al LLM
    → TTL configurable por agente
    → Output: respuesta cacheada O continúa
        │
        ▼
[4] MODEL ROUTER
    → Evalúa estrategia: privacy/cost/quality/speed
    → Clasifica complejidad de la tarea
    → Selecciona modelo óptimo
    → Verifica rate limits y presupuesto
    → Output: modelo seleccionado
        │
        ▼
[5] LLM CALL
    → Con retry logic y circuit breaker
    → Timeout configurable
    → Fallback automático si falla
        │
        ▼
[6] OUTPUT VALIDATOR
    → Valida estructura del output
    → Detecta PII en respuesta
    → Sanitiza antes de retornar
        │
        ▼
[7] CACHE STORE + METRICS
    → Guarda en cache si aplica
    → Registra: tokens, coste, latencia, modelo
OUTPUT (respuesta validada + métricas)
```

---

## 7. STACK TECNOLÓGICO COMPLETO

| Capa | Tecnología | Justificación |
|------|-----------|---------------|
| Backend | FastAPI 0.111+ | Async nativo, OpenAPI auto-docs, tipado |
| ORM | SQLAlchemy 2.x async | Mejor ORM Python, async first |
| Migrations | Alembic | Standard, integrado con SQLAlchemy |
| Frontend | React 18 + TypeScript | Comunidad, tipado, ecosystem |
| Build | Vite | Velocidad de desarrollo |
| State | Zustand | Simple, sin boilerplate |
| DB principal | PostgreSQL 15 (JSONB) | ACID + flexibilidad JSON para configs |
| Cache / Queue | Redis 7 | Sessions, cache semántico, Celery broker |
| Vector DB | Qdrant | Embeddings para cache semántico y RAG |
| Jobs | Celery 5 | Retry, monitoring, dead letter queue |
| LLM abstraction | LiteLLM | 100+ providers, un solo contrato de API |
| Evals | promptfoo CLI | Industry standard, red teaming incluido |
| Auth | JWT + python-jose | Stateless, estándar |
| Logs | structlog (JSON) | Structured, queryable |
| Metrics | Prometheus + Grafana | Industry standard |
| Traces | OpenTelemetry | Vendor-agnostic tracing |
| Container | Docker + docker-compose | Reproducibilidad |
| CI/CD | GitHub Actions | Integrado, free tier suficiente |
| Security scan | Dependabot + Trivy | Vulnerabilidades automáticas |

---

## 8. ESTRUCTURA DE CARPETAS COMPLETA

```
agent-factory-core/
├── CLAUDE.md                          ← ESTE FICHERO
├── HANDOFF.md                         ← Contexto completo de decisiones
├── ARCHITECTURE.md                    ← Diagramas y ADRs
├── PRESENTATION.md                    ← Presentación ejecutiva para cliente
├── README.md                          ← Setup y quickstart
├── CONTRIBUTING.md                    ← Guía de contribución
├── docker-compose.yml
├── docker-compose.prod.yml
├── .env.example
├── .gitignore
│
├── backend/
│   ├── app/
│   │   ├── main.py                    ← FastAPI entry point
│   │   ├── config.py                  ← pydantic-settings
│   │   ├── database.py                ← SQLAlchemy async engine
│   │   ├── auth.py                    ← JWT + RBAC middleware
│   │   ├── api/
│   │   │   ├── agents.py              ← CRUD agentes
│   │   │   ├── skills.py              ← CRUD skills
│   │   │   ├── gems.py                ← CRUD gemas ← NUEVO
│   │   │   ├── evals.py               ← Evaluaciones
│   │   │   └── exports.py             ← Exportadores
│   │   ├── models/
│   │   │   ├── agent.py
│   │   │   ├── skill.py
│   │   │   ├── gem.py                 ← NUEVO
│   │   │   └── eval_result.py
│   │   ├── schemas/
│   │   │   ├── agent.py
│   │   │   ├── skill.py
│   │   │   └── gem.py                 ← NUEVO
│   │   ├── services/
│   │   │   ├── agent_registry.py
│   │   │   ├── skill_registry.py
│   │   │   ├── gem_registry.py        ← NUEVO
│   │   │   ├── llm_router.py
│   │   │   ├── optimization_engine.py ← NUEVO (core del sistema)
│   │   │   ├── eval_runner.py
│   │   │   └── exporter.py
│   │   ├── optimization/              ← NUEVO módulo completo
│   │   │   ├── __init__.py
│   │   │   ├── prompt_optimizer.py
│   │   │   ├── context_compressor.py
│   │   │   ├── response_cache.py      ← cache semántico con Qdrant
│   │   │   ├── model_selector.py
│   │   │   ├── batch_processor.py
│   │   │   └── cost_tracker.py
│   │   ├── security/
│   │   │   ├── pii_detector.py
│   │   │   ├── tool_sandbox.py
│   │   │   ├── rate_limiter.py
│   │   │   └── audit_logger.py
│   │   └── utils/
│   │       ├── logger.py
│   │       ├── validators.py
│   │       └── token_counter.py       ← NUEVO
│   │
│   ├── skills/                        ← 15 skills implementadas
│   │   ├── base.py                    ← BaseSkill abstract class
│   │   ├── optimization/
│   │   │   ├── prompt_optimization.py
│   │   │   ├── context_compression.py
│   │   │   ├── model_router.py
│   │   │   ├── response_cache.py
│   │   │   └── skill_recommender.py
│   │   ├── security/
│   │   │   ├── prompt_injection_defense.py
│   │   │   ├── tool_safety.py
│   │   │   ├── output_validation.py
│   │   │   ├── human_approval.py
│   │   │   └── error_recovery.py
│   │   └── governance/
│   │       ├── cost_guard.py
│   │       ├── logging_observability.py
│   │       ├── eval_generation.py
│   │       ├── promptfoo_test_runner.py
│   │       └── memory_policy.py
│   │
│   ├── gems/                          ← NUEVO: Gem templates base
│   │   ├── identity/
│   │   │   └── gem_professional_tone.yaml
│   │   ├── security/
│   │   │   └── gem_base_restrictions.yaml
│   │   └── knowledge/
│   │       └── gem_template.yaml
│   │
│   ├── exporters/
│   │   ├── base.py
│   │   ├── json_exporter.py
│   │   ├── yaml_exporter.py
│   │   ├── docker_exporter.py
│   │   ├── paquete_exporter.py
│   │   └── navaja_exporter.py
│   │
│   ├── jobs/
│   │   ├── celery_app.py
│   │   ├── eval_tasks.py
│   │   ├── export_tasks.py
│   │   ├── optimization_tasks.py      ← NUEVO
│   │   └── cache_tasks.py             ← NUEVO
│   │
│   ├── migrations/versions/
│   ├── tests/
│   │   ├── conftest.py
│   │   ├── test_agents.py
│   │   ├── test_skills.py
│   │   ├── test_gems.py               ← NUEVO
│   │   ├── test_optimization.py       ← NUEVO
│   │   └── test_exporters.py
│   │
│   ├── requirements.txt
│   ├── Dockerfile
│   └── alembic.ini
│
├── frontend/
│   └── src/
│       ├── components/
│       │   ├── AgentWizard/           ← Wizard 5 pasos (no 4 — añadir Gems)
│       │   ├── GemManager/            ← NUEVO: Gestión de gemas
│       │   ├── OptimizationDashboard/ ← NUEVO: Métricas de optimización
│       │   ├── CostTracker/           ← NUEVO: Costes en tiempo real
│       │   ├── AgentEditor.tsx
│       │   ├── SkillManager.tsx
│       │   └── Dashboard.tsx
│       └── pages/
│           ├── Home.tsx
│           ├── AgentDetail.tsx
│           ├── EvalResults.tsx
│           ├── GemLibrary.tsx         ← NUEVO
│           ├── OptimizationReport.tsx ← NUEVO
│           └── Login.tsx
│
├── templates/
│   ├── agents/
│   │   ├── customer_support.yaml
│   │   ├── code_reviewer.yaml
│   │   ├── research_analyst.yaml
│   │   └── data_processor.yaml       ← NUEVO (batch processing)
│   ├── gems/                          ← NUEVO
│   │   ├── gem_professional_tone.yaml
│   │   ├── gem_security_baseline.yaml
│   │   └── gem_template.yaml
│   └── promptfoo/
│       └── base_test_suite.yaml
│
├── docs/
│   ├── index.md
│   ├── architecture.md
│   ├── schema.v1.yaml                 ← Schema completo (agents + gems)
│   ├── gems.md                        ← NUEVO: Especificación de gemas
│   ├── optimization.md                ← NUEVO: Optimization Engine spec
│   ├── skills-catalog.md              ← NUEVO: Las 15 skills documentadas
│   ├── api.md
│   ├── security-threat-model.md
│   └── deployment.md
│
└── .github/
    └── workflows/
        ├── test.yml
        ├── security.yml
        └── deploy-staging.yml
```

---

## 9. REGLAS DE DESARROLLO — NO NEGOCIABLES

### Código Python
1. **Tipado estricto**: mypy en modo strict. Nunca `Any` sin comentario justificado.
2. **Async everywhere**: Todas las rutas FastAPI y métodos de servicios son `async def`.
3. **Pydantic siempre**: Todos los inputs/outputs tienen schema Pydantic. Cero dicts crudos.
4. **Logging estructurado**: structlog con campos contextuales. Nunca `print()`.
5. **Tests primero**: cada nueva función = test en el mismo PR. Target: 80%+ coverage.
6. **Migrations siempre**: cualquier cambio de schema DB = nueva migración Alembic.
7. **Secrets en env**: nunca hardcodeados. Si ves uno, es un bug crítico.

### Arquitectura
1. **Optimization Engine siempre en el pipeline**: ninguna llamada LLM bypasea el engine.
2. **Gems por referencia, nunca por valor**: nunca copiar el contenido de una gem en un agente.
3. **Skills composables**: una skill no puede llamar a otra. Composición vía AgentProfile.
4. **LLM Router es el único punto de contacto con proveedores**: nada más llama a LiteLLM.
5. **Security en cada capa**: middleware + router + cada servicio. No en un único módulo.

### Git
- Branches: `feature/`, `fix/`, `docs/`, `chore/`, `refactor/`
- Commits: Conventional Commits (`feat:`, `fix:`, `docs:`, `perf:`, `test:`)
- PRs: descripción + tests + sin CI roto. Nunca merge sin verde.
- Main: siempre deployable. Si rompiste main, es P0.

---

## 10. VARIABLES DE ENTORNO REQUERIDAS

```bash
# Base de datos
DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/agentfactory
REDIS_URL=redis://localhost:6379/0
QDRANT_URL=http://localhost:6333

# LLMs (añade solo los que uses)
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GROQ_API_KEY=gsk_...
GEMINI_API_KEY=...
OLLAMA_BASE_URL=http://localhost:11434

# Auth
SECRET_KEY=genera-con-openssl-rand-hex-32
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440

# Optimización
CACHE_TTL_SECONDS=3600
CACHE_SEMANTIC_THRESHOLD=0.92
CONTEXT_COMPRESSION_THRESHOLD=0.70
DEFAULT_DAILY_COST_BUDGET_USD=10.0
DEFAULT_RATE_LIMIT_PER_MINUTE=60

# App
ENVIRONMENT=development
LOG_LEVEL=INFO
ALLOWED_ORIGINS=http://localhost:3000
EVAL_MIN_SCORE=0.85
```

---

## 11. ROADMAP ACTIVO — 9 SEMANAS

### FASE 1: Foundation (Semanas 1-3) ← EN CURSO
- [ ] S1.1: Setup + FastAPI boilerplate + observabilidad (Prometheus, structlog, OTel)
- [ ] S1.2: Agent model + CRUD API + schema YAML v1 (con gems y optimization)
- [ ] S1.3: Skill system (15 skills, BaseSkill, SkillRegistry)
- [ ] S1.4: Gem Registry (GemModel, CRUD, resolución de referencias)

### FASE 2: Optimization + Eval (Semanas 4-6)
- [ ] S2.1: Optimization Engine completo (los 7 pasos del pipeline)
- [ ] S2.2: LLM Router con RoutingStrategy y circuit breaker
- [ ] S2.3: promptfoo integration + auto test generation + score gate
- [ ] S2.4: Cost Tracker dashboard en tiempo real

### FASE 3: Producción (Semanas 7-9)
- [ ] S3.1: Security hardening (PII, audit, tool sandbox)
- [ ] S3.2: Frontend completo (Wizard 5 pasos, Gem Manager, Cost Dashboard)
- [ ] S3.3: Exportadores (Paquete, Navaja Suiza, Docker, JSON/YAML)
- [ ] S3.4: Docs + launch

---

## 12. DECISIONES ARQUITECTÓNICAS (ADRs)

| # | Decisión | Elegido | Rechazado | Razón |
|---|----------|---------|-----------|-------|
| 1 | Backend framework | FastAPI | Django, Flask | Async nativo, OpenAPI auto-docs |
| 2 | LLM abstraction | LiteLLM | Custom, LangChain | 100+ providers, un contrato |
| 3 | Cache semántico | Qdrant + Redis | Solo Redis | Similarity search para cache |
| 4 | Job queue | Celery | RQ, FastAPI BG | Retry, DLQ, monitoring |
| 5 | Eval framework | promptfoo | Langfuse, custom | Red teaming + CI/CD nativo |
| 6 | MCP position | Adaptador externo | Core | Riesgos STDIO, Fase 4 |
| 7 | Agent storage | PostgreSQL JSONB | MongoDB | ACID + flexibilidad JSON |
| 8 | Gem storage | PostgreSQL + Files | Git submodules | Query + versionado |
| 9 | Optimization | Pipeline propio | LangChain | Control total, sin dependencias |
| 10 | Provider agnosticism | LiteLLM | Abstracciones propias | Mantenibilidad |

---

## 13. PREGUNTAS PENDIENTES PARA BORJA — NO IMPLEMENTAR SIN RESPUESTA

1. **Formato Paquete**: ¿Qué estructura espera el exporter? ¿JSON config? ¿Plugin Python? ¿Directorio con archivos?
2. **Formato Navaja Suiza**: ¿Qué formato acepta? ¿API REST? ¿CLI? ¿YAML?
3. **Multi-tenancy MVP**: ¿Es para uso personal (1 usuario) o múltiples usuarios desde el inicio?
4. **Hosting inicial**: ¿Local, VPS propio, Railway, Fly.io?
5. **Autenticación**: ¿Token simple (1 usuario) o sistema de registro completo?
6. **Gems compartidas**: ¿Las gems son privadas por usuario o hay una librería global compartida?
7. **Presupuesto LLM**: ¿Cuál es el límite diario de gasto aceptable en producción?

---

## 14. COMANDOS ÚTILES

```bash
# Setup inicial
bash scripts/setup_local.sh

# Backend dev
cd backend && uvicorn app.main:app --reload --port 8000

# Frontend dev
cd frontend && npm run dev

# Tests con coverage
cd backend && pytest tests/ -v --cov=app --cov-report=html --cov-fail-under=80

# Tipo checking
cd backend && mypy app/ --strict

# Migrations
cd backend && alembic upgrade head
cd backend && alembic revision --autogenerate -m "descripcion"

# Docker completo
docker-compose up -d
docker-compose logs -f backend

# Celery worker
cd backend && celery -A jobs.celery_app worker --loglevel=info

# Evaluaciones
npx promptfoo eval --config templates/promptfoo/base_test_suite.yaml

# Token counter (debug)
cd backend && python -m utils.token_counter --agent customer_support_v1
```

---

## 15. LEE TAMBIÉN (en este orden)

1. `HANDOFF.md` — Historia, decisiones y contexto completo
2. `ARCHITECTURE.md` — Diagramas detallados y ADRs
3. `docs/schema.v1.yaml` — Schema oficial (fuente de verdad de agentes y gems)
4. `docs/gems.md` — Especificación del sistema de gemas
5. `docs/optimization.md` — Especificación del Optimization Engine
6. `docs/skills-catalog.md` — Las 15 skills documentadas
7. `PRESENTATION.md` — Presentación ejecutiva para cliente
