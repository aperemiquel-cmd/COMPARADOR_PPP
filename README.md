# COMPARADOR_PPP — Biocontrol Technologies

> Experimento comparativo **Claude Code vs. Codex** para la construcción de un aplicativo web de comparación de productos fitosanitarios (PPP).

---

## ¿Qué es este proyecto?

El **Comparador PPP** es una aplicación web interna que permite al equipo de I+D, Regulatory y Comercial consultar y comparar parámetros técnicos de productos biofungicidas registrados en múltiples países, exportando los resultados en PDF y Excel.

Este repositorio estructura un **ensayo experimental controlado** en el que cuatro configuraciones de agentes de IA construyen el mismo aplicativo partiendo de idéntica especificación, para determinar qué enfoque ofrece mejor relación calidad / coste / tiempo.

---

## Casos del experimento

| Caso | Configuración | Carpeta |
|------|--------------|---------|
| A | Claude Code — agente único | `/case-A/` |
| B | Codex — agente único | `/case-B/` |
| C | Claude Code gestor + Codex revisor | `/case-C/` |
| D | Codex gestor + Claude Code revisor | `/case-D/` |

---

## Estructura del repositorio

```
COMPARADOR_PPP/
├── README.md
├── .gitignore
├── data/                          ← Fuente de verdad (solo lectura para agentes)
│   ├── FOT_COMPARADOR_PPPv11.csv  ← Base de datos de productos (44 registros)
│   ├── schema_mysql.sql           ← Schema MySQL
│   └── seed_mysql.sql             ← Seed con los 44 registros
├── docs/                          ← Especificaciones (solo lectura para agentes)
│   ├── SPEC_APLICATIVO.md         ← Especificación funcional completa
│   ├── STACK.md                   ← Stack y requisitos de despliegue
│   ├── RUBRICA_EVALUACION.md      ← Los 3 paquetes de evaluación
│   ├── EXPORT_REFERENCE.png       ← Referencia visual del formato de exportación
│   └── Diseno_Experimental_v1.0.docx
├── case-A/                        ← Claude Code solo
├── case-B/                        ← Codex solo
├── case-C/                        ← Claude Code gestor + Codex revisor
├── case-D/                        ← Codex gestor + Claude Code revisor
└── evaluation/
    └── COMPARATIVA_RESULTADOS.md
```

---

## Stack tecnológico

| Capa | Tecnología |
|------|-----------|
| Servidor | Plesk (Linux) |
| Base de datos | MySQL 8.x (admin vía phpMyAdmin) |
| Backend | PHP 8.x — API REST |
| Export PDF | mPDF / TCPDF |
| Export Excel | PhpSpreadsheet |
| Auth | Sesiones PHP + tabla `users` (roles: viewer / regulatory / admin) |

---

## Documentación

- 📄 **Diseño experimental completo:** `docs/Diseno_Experimental_v1.0.docx`
- 📋 **Especificación funcional:** `docs/SPEC_APLICATIVO.md`
- 📊 **Rúbrica de evaluación:** `docs/RUBRICA_EVALUACION.md`

---

*Biocontrol Technologies · I+D y Desarrollo · Uso exclusivo interno*
