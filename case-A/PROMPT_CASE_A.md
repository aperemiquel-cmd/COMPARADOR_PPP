# PROMPT — Caso A: Claude Code (agente único)

> Copiar y ejecutar este prompt completo en Claude Code al inicio de la sesión.  
> Registrar el timestamp exacto de inicio antes de enviarlo.

---

## PROMPT

```
CONTEXTO DEL PROYECTO:
Lee y comprende completamente los siguientes archivos ANTES de escribir ningún código:
  - /docs/SPEC_APLICATIVO.md     → especificación funcional completa
  - /docs/STACK.md               → stack y entorno de despliegue
  - /docs/RUBRICA_EVALUACION.md  → criterios con los que será evaluado tu trabajo
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
Eres el único desarrollador de este proyecto. Construye el aplicativo Comparador PPP 
completo, desplegable en Plesk, siguiendo estrictamente la especificación.

ENTREGABLES REQUERIDOS (en orden de prioridad):
  1. data/schema_mysql.sql  — schema MySQL compatible con los campos del CSV
  2. data/seed_mysql.sql    — seed con los 44 registros del CSV
  3. backend/               — API PHP con todos los endpoints de SPEC_APLICATIVO.md §4
  4. frontend/              — interfaz web con las 6 pantallas de SPEC_APLICATIVO.md §2
  5. INSTALL.md             — instrucciones paso a paso de despliegue en Plesk
  6. RESULTS_A.md           — log de decisiones de diseño y observaciones

RESTRICCIONES:
  - Sin sistema de estrellas ni puntuaciones en ninguna parte de la interfaz
  - Parámetros comparables: microorganismo y cepa, formulación, concentración, dosis,
    almacenamiento, titular/comercializador, país
  - Interfaz disponible en 3 idiomas: ES (Castellano), EN (English), CA (Català)
  - Todas las queries usan prepared statements
  - El directorio /data/ no accesible desde la web (.htaccess)

Trabaja de forma autónoma hasta completar todos los entregables.
Documenta cada decisión de diseño relevante en RESULTS_A.md conforme avances.
```

---

## Registro de ejecución

| Campo | Valor |
|-------|-------|
| Timestamp inicio | |
| Timestamp fin | |
| Nº de iteraciones (prompts adicionales) | |
| Tokens entrada totales | |
| Tokens salida totales | |
| Coste estimado (€) | |
| Tiempo hasta 1er build ejecutable | |

---

*Ver `docs/RUBRICA_EVALUACION.md` para la plantilla completa de RESULTS_A.md*
