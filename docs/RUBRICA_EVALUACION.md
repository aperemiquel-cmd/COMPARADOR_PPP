# RUBRICA_EVALUACION.md — Rúbrica de evaluación del experimento

> Los tres paquetes son **independientes**. Se presentan por separado en la comparativa final — no se ponderan entre ellos.

---

## Paquete 1 — Calidad del producto (100 puntos)

| # | Criterio | Peso | Cómo medirlo |
|---|---------|------|-------------|
| 1 | **Completitud funcional** | 20% | % de pantallas y features del `SPEC_APLICATIVO.md` implementadas y funcionando. |
| 2 | **Calidad del código** | 15% | Revisión manual: estructura de archivos, separación de capas (MVC), validación de inputs, prepared statements, ausencia de código muerto. |
| 3 | **Fidelidad al FOT** | 15% | ¿Los datos del CSV se muestran correctamente? ¿Todos los campos presentes? ¿Sin truncados ni errores de encoding? |
| 4 | **Exportación funcional** | 10% | PDF tabla y PDF ficha generan sin errores. Excel descargable y abre correctamente. Filtros aplicados en el export. |
| 5 | **Compatibilidad BD** | 5% | Schema MySQL correcto y ejecutable. Seed completo (44 registros). Queries sin errores en MySQL 8.x. |
| 6 | **Velocidad de carga / respuesta** | 5% | Time to first meaningful paint (vista catálogo completa). Tiempo de respuesta de filtros. **Medición:** DevTools, red 4G simulada (throttling), misma máquina para todos los casos. |
| 7 | **Peso total del aplicativo** | 5% | Suma de assets descargados en carga inicial (HTML + CSS + JS + fuentes + imágenes). **Medición:** pestaña Network de DevTools, sin caché, primera carga. |
| 8 | **UX / fidelidad visual** | 5% | Proximidad al formato de `/docs/EXPORT_REFERENCE.png`. Legibilidad de la tabla comparativa. Claridad de filtros y botones de exportación. |

**Escala de puntuación por criterio:**
- 100% del peso → implementado correctamente y sin errores.
- 50% del peso → implementado con deficiencias menores.
- 0% del peso → no implementado o con errores críticos.

---

## Paquete 2 — Coste en tokens

> Registrar desde los logs de la API o interfaz del agente. No estimaciones.

| Métrica | Qué registrar |
|---------|--------------|
| Tokens de entrada totales | Suma de todos los tokens de entrada: prompt inicial + contexto acumulado + iteraciones adicionales. |
| Tokens de salida totales | Suma de todos los tokens de salida: código generado + respuestas de texto en toda la sesión. |
| Tokens por feature entregada | `(Tokens totales) / (nº de features del spec implementadas)`. Coste unitario por funcionalidad. |
| Coste estimado (€) | Aplicar tarifa vigente de cada modelo en la fecha de ejecución. Registrar por separado Claude Code y Codex. |
| Casos C y D (colaborativos) | Registrar por agente: gestor / revisor / total del caso. |

---

## Paquete 3 — Tiempo de ejecución

> Registrar timestamps exactos. No estimaciones.

| Métrica | Qué registrar |
|---------|--------------|
| Tiempo total wall-clock | Desde timestamp del primer prompt hasta el último commit con el aplicativo funcional y desplegable. |
| Tiempo hasta primer build ejecutable | Primer momento en que el aplicativo arranca sin errores críticos (aunque funcionalidades estén incompletas). |
| Número de iteraciones | Prompts adicionales al inicial necesarios para corregir errores, aclarar requisitos o desbloquear al agente. |
| Tiempo de handoff (solo C y D) | Tiempo desde que el agente gestor finaliza hasta que el agente revisor entrega la versión mejorada. |

---

## Plantilla de resultados por caso

Copiar en el `RESULTS_X.md` de cada caso al finalizar:

```
## Métricas — Caso [A/B/C/D]

### Paquete 1 — Calidad del producto
- Completitud funcional (/20): 
- Calidad del código (/15): 
- Fidelidad al FOT (/15): 
- Exportación funcional (/10): 
- Compatibilidad BD (/5): 
- Velocidad de carga (/5): [tiempo TTFP: __ ms | tiempo filtros: __ ms]
- Peso del aplicativo (/5): [__ KB / __ MB]
- UX / fidelidad visual (/5): 
- **TOTAL P1 (/100):** 

### Paquete 2 — Coste en tokens
- Tokens entrada: 
- Tokens salida: 
- Tokens/feature: 
- Coste estimado (€): 

### Paquete 3 — Tiempo
- Timestamp inicio: 
- Timestamp fin: 
- Tiempo total: 
- Tiempo 1er build ejecutable: 
- Nº de iteraciones: 
[Solo C/D] - Tiempo de handoff: 
```

---

*Biocontrol Technologies · Documento interno · v1.0 · Mayo 2026*
