# PROMPTS — Caso C: Claude Code (gestor) + Codex (revisor)

> Dos prompts secuenciales. Primero se ejecuta el prompt del gestor (Claude Code).  
> Solo cuando Claude Code declare el trabajo finalizado se ejecuta el prompt del revisor (Codex).

---

## PASO 1 — PROMPT GESTOR: Claude Code

> Registrar timestamp de inicio antes de enviarlo.

```
CONTEXTO DEL PROYECTO:
Lee y comprende completamente los siguientes archivos ANTES de escribir ningún código:
  - /docs/SPEC_APLICATIVO.md     → especificación funcional completa
  - /docs/STACK.md               → stack y entorno de despliegue
  - /docs/RUBRICA_EVALUACION.md  → criterios con los que será evaluado el proyecto
  - /data/FOT_COMPARADOR_PPPv11.csv → base de datos de productos (fuente de verdad)
  - /docs/EXPORT_REFERENCE.png   → referencia visual del formato de exportación

ENTORNO DE DESPLIEGUE:
  - Servidor: Plesk (Linux)
  - Base de datos: MySQL 8.x (administración vía phpMyAdmin — solo equipo interno)
  - Backend: PHP 8.x — API REST en /api/
  - Export PDF: mPDF o TCPDF
  - Export Excel: PhpSpreadsheet
  - Auth: sesiones PHP + tabla users (roles: viewer / regulatory / admin)
  - Sin registro público — usuarios creados manualmente por admin

MISIÓN:
Eres el arquitecto y desarrollador principal de este proyecto. Construye el aplicativo 
Comparador PPP completo siguiendo estrictamente la especificación.

ENTREGABLES REQUERIDOS:
  1. data/schema_mysql.sql  — schema MySQL compatible con los campos del CSV
  2. data/seed_mysql.sql    — seed con los 44 registros del CSV
  3. backend/               — API PHP con todos los endpoints de SPEC_APLICATIVO.md §4
  4. frontend/              — interfaz web con las 6 pantallas de SPEC_APLICATIVO.md §2
  5. INSTALL.md             — instrucciones paso a paso de despliegue en Plesk

RESTRICCIONES:
  - Sin sistema de estrellas ni puntuaciones en ninguna parte de la interfaz
  - Parámetros comparables: microorganismo y cepa, formulación, concentración, dosis,
    almacenamiento, titular/comercializador, país
  - Interfaz disponible en 3 idiomas: ES, EN, CA
  - Todas las queries usan prepared statements
  - El directorio /data/ no accesible desde la web (.htaccess)

NOTA IMPORTANTE — TRABAJO COLABORATIVO:
Tu código será revisado por un segundo agente (Codex) que optimizará compatibilidad
y realizará refactoring. Para facilitar esa revisión:
  - Escribe código modular con interfaces claras entre capas (MVC)
  - Comenta las decisiones de diseño no evidentes directamente en el código
  - Al finalizar, documenta en RESULTS_C.md:
    · Decisiones tomadas y su justificación
    · Áreas donde anticipas mejoras o revisiones necesarias
    · Dependencias entre módulos que el revisor debe conocer
    · Cualquier deuda técnica pendiente

Trabaja de forma autónoma hasta completar todos los entregables.
```

---

## Registro ejecución — Gestor (Claude Code)

| Campo | Valor |
|-------|-------|
| Timestamp inicio gestor | |
| Timestamp fin gestor | |
| Nº de iteraciones gestor | |
| Tokens entrada gestor | |
| Tokens salida gestor | |
| Coste gestor (€) | |

---

## PASO 2 — PROMPT REVISOR: Codex

> Ejecutar SOLO después de que Claude Code haya finalizado y hecho commit.  
> Registrar timestamp de inicio del revisor.

```
CONTEXTO DEL PROYECTO:
Lee y comprende completamente los siguientes archivos ANTES de modificar nada:
  - /docs/SPEC_APLICATIVO.md     → especificación funcional completa (referencia de verdad)
  - /docs/STACK.md               → stack y entorno de despliegue
  - /case-C/RESULTS_C.md         → decisiones del agente anterior (LEER PRIMERO)
  - /data/FOT_COMPARADOR_PPPv11.csv → base de datos de productos

MISIÓN:
Recibes un proyecto ya construido por Claude Code en /case-C/.
Tu misión es revisarlo y mejorarlo. NO lo reescribas desde cero.

PROCESO OBLIGATORIO ANTES DE TOCAR NADA:
  1. Lee RESULTS_C.md para entender las decisiones del agente anterior
  2. Lee SPEC_APLICATIVO.md para identificar qué funcionalidades faltan o están mal
  3. Audita el código: compatibilidad, tipado PHP, queries, seguridad, reutilización

TUS TAREAS (en orden estricto de prioridad):
  1. Corregir errores que impidan la ejecución del proyecto
  2. Mejorar compatibilidad frontend ↔ backend (contratos de API, formatos de respuesta)
  3. Refactorizar para maximizar reutilización de componentes y funciones
  4. Completar funcionalidades faltantes según SPEC_APLICATIVO.md
  5. Mejorar la seguridad si detectas vulnerabilidades (SQL injection, XSS, etc.)

DOCUMENTACIÓN:
Añade una sección "## Revisión Codex" en RESULTS_C.md indicando:
  - Qué errores encontraste y corregiste
  - Qué refactorizaciones realizaste y por qué
  - Qué funcionalidades completaste
  - Qué no pudiste mejorar y por qué

NO reescribas desde cero. Transforma y mejora lo que existe.
```

---

## Registro ejecución — Revisor (Codex)

| Campo | Valor |
|-------|-------|
| Timestamp inicio revisor | |
| Timestamp fin revisor | |
| Tiempo de handoff (fin gestor → inicio revisor) | |
| Nº de iteraciones revisor | |
| Tokens entrada revisor | |
| Tokens salida revisor | |
| Coste revisor (€) | |

---

*Ver `docs/RUBRICA_EVALUACION.md` para la plantilla completa de métricas*
