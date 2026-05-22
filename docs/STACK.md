# STACK.md — Entorno de despliegue y stack tecnológico

> **SOLO LECTURA PARA AGENTES.** Este documento define el entorno exacto en el que debe desplegarse el aplicativo. Todos los casos del experimento comparten este mismo stack.

---

## Entorno de despliegue

| Parámetro | Valor |
|-----------|-------|
| Servidor | Plesk (Linux) |
| Base de datos | MySQL 8.x |
| Admin BD | phpMyAdmin (solo equipo interno, no expuesto al usuario final) |
| Backend | PHP 8.x |
| Acceso | Aplicativo web con autenticación propia. Sin registro público. Usuarios creados manualmente por admin. |

---

## Stack obligatorio

| Capa | Tecnología | Notas |
|------|-----------|-------|
| Backend API | PHP 8.x — REST en `/api/` | Endpoints: `GET /api/products`, `GET /api/export` |
| Base de datos | MySQL 8.x | Schema y seed en `/data/` |
| Export PDF | mPDF o TCPDF | Tabla comparativa + ficha de producto |
| Export Excel | PhpSpreadsheet | `.xlsx` descargable |
| Autenticación | Sesiones PHP + tabla `users` | Roles: viewer / regulatory / admin |
| Frontend | HTML + CSS + JS (React opcional) | Decisión libre del agente |

---

## Roles de acceso

| Rol | Permisos |
|-----|---------|
| `viewer` | Consulta y exportación de datos con estado **Confirmado** únicamente. Sin notas internas ni historial de validación. |
| `regulatory` | Todos los datos (Confirmado + Parcial + Pendiente) + notas internas + alertas de caducidad. |
| `admin` | Todo lo anterior + gestión de maestros (formulaciones, países, estados) + gestión de usuarios. |

---

## Instrucciones de despliegue en Plesk

El agente debe incluir un archivo `INSTALL.md` en su carpeta de caso con los pasos exactos:

1. Subir los archivos por FTP o Git al directorio web del dominio en Plesk.
2. Crear la base de datos MySQL desde phpMyAdmin o el panel de Plesk.
3. Ejecutar `data/schema_mysql.sql` para crear las tablas.
4. Ejecutar `data/seed_mysql.sql` para cargar los 44 registros.
5. Configurar el archivo `.env` o `config.php` con las credenciales de la BD.
6. Crear el primer usuario admin desde la consola o un script de setup.
7. Verificar que `GET /api/products` devuelve datos correctamente.

---

## Restricciones técnicas

- **Sin sistema de estrellas** en ninguna parte de la interfaz.
- Los parámetros comparables son: microorganismo y cepa, formulación, concentración, dosis, almacenamiento, titular/comercializador, país.
- La interfaz debe estar disponible en **3 idiomas**: Castellano (ES), English (EN), Català (CA). Solo se traducen cabeceras y etiquetas; los valores técnicos son invariables.
- Las queries a MySQL deben usar **prepared statements** para evitar inyección SQL.
- El acceso a phpMyAdmin es exclusivo del equipo técnico — no debe vincularse ni mencionarse en la interfaz del aplicativo.

---

*Biocontrol Technologies · Documento interno · v1.0 · Mayo 2026*
