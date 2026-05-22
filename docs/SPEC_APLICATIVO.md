# SPEC_APLICATIVO.md — Especificación funcional del Comparador PPP

> **FUENTE DE VERDAD PARA LOS AGENTES.** Esta especificación define qué debe construirse. Cualquier duda de implementación se resuelve consultando este documento y los archivos de `/data/`.

---

## 1. Descripción general

El **Comparador PPP** es una aplicación web interna de consulta y exportación sobre la base de datos de biofungicidas fitosanitarios de Biocontrol Technologies. Permite comparar parámetros técnicos de productos registrados en múltiples países y exportar los resultados en formatos estructurados.

**Acceso:** Solo usuarios verificados con cuenta activa. Sin autoregistro.  
**Usuarios objetivo:** Equipo de I+D, Regulatory y Comercial.  
**Datos fuente:** `/data/FOT_COMPARADOR_PPPv11.csv` — 44 registros, 24 productos, 16 países.

---

## 2. Pantallas requeridas

### 2.1 Login
- Formulario de usuario y contraseña.
- Redirección según rol tras autenticación.
- Mensaje de error claro si credenciales incorrectas.
- Sin "Recordarme" ni recuperación de contraseña en el MVP.

### 2.2 Dashboard (página de inicio)
- KPIs globales visibles de un vistazo:
  - Total de productos en la BD
  - % de registros con estado Confirmado
  - Nº de registros con estado Parcial o Pendiente
  - Nº de países cubiertos
- Acceso rápido a las 3 vistas principales: Catálogo, Comparación, Exportación.
- Solo rol `regulatory` y `admin` ven el KPI de Parcial/Pendiente.

### 2.3 Catálogo de productos
- Listado completo de productos con los siguientes filtros activos (combinables):
  - País (selección múltiple)
  - Microorganismo / género (Trichoderma, Bacillus, Pythium…)
  - Formulación (WP, WG, SC, SP, GR…)
  - Titular / comercializador
  - Estado del dato (Confirmado / Parcial / Pendiente / Cancelado / No registrado)
  - Tier (Tier 1 / Tier 2)
- Vista tabla con columnas: Producto, Microorganismo y cepa, País, Formulación, Concentración, Estado.
- Cada fila es seleccionable para añadir a la comparación.
- Rol `viewer`: solo ve registros con estado Confirmado.

### 2.4 Vista de comparación
- Selección de 2 a 6 productos para comparar lado a lado.
- Tabla comparativa con los siguientes parámetros como filas:
  - Microorganismo y cepa
  - Formulación
  - Concentración
  - Dosis
  - Almacenamiento
  - Titular / Comercializador
  - País de registro
- Formato visual: ver `/docs/EXPORT_REFERENCE.png` como referencia de estilo.
- **Sin sistema de estrellas.** Sin puntuaciones numéricas.
- Botón "Exportar esta comparación" que lanza el motor de exportación.

### 2.5 Motor de exportación
El usuario configura la exportación con los siguientes parámetros:

**Filtros disponibles:**
- Por Tier (Tier 1 / Tier 2 / Ambos)
- Por país (selección múltiple)
- Por estado (Confirmado / Parcial / Pendiente / Cancelado / No registrado)
- Por microorganismo (género o cepa específica)
- Por titular / casa comercial

**Formatos de exportación:**

| Formato | Descripción |
|---------|-------------|
| PDF — Tabla comparativa | Tabla con los productos seleccionados y sus parámetros. Logotipo Biocontrol Technologies en cabecera. |
| PDF — Ficha de producto | Una página por producto con todos sus campos y fuente de datos. |
| Excel (.xlsx) | Hoja única o por país según el filtro activo. Colores por estado (Confirmado=verde, Parcial=naranja, Pendiente=gris, Cancelado=rojo). |

**Idiomas de exportación:** ES (Castellano) / EN (English) / CA (Català).  
Solo se traducen cabeceras y etiquetas. Los valores técnicos (concentración, cepa, etc.) son invariables.

### 2.6 Panel de administración (solo rol `admin`)
- Gestión de usuarios: crear, desactivar, cambiar rol.
- Gestión de maestros: formulaciones, países, estados del dato.
- Vista de solo lectura del changelog de la BD.

---

## 3. Estructura de la base de datos

La BD se construye desde `/data/FOT_COMPARADOR_PPPv11.csv`. Las tablas mínimas requeridas:

### Tabla `products`
Campos principales (mapeo desde el CSV):

| Campo CSV | Campo BD | Tipo |
|-----------|---------|------|
| ID | product_id | VARCHAR(10) PK |
| Tier | tier | ENUM('Tier 1','Tier 2') |
| Producto / Familia comercial | product_name | VARCHAR(255) |
| Microorganismo y cepa | organism_strain | TEXT |
| Origen | origin | VARCHAR(100) |
| Formulación | formulation | VARCHAR(50) |
| Comercializador / Titular | titular | VARCHAR(255) |
| País | country_code | CHAR(2-3) |
| Almacenamiento | storage | TEXT |
| Concentración | concentration | TEXT |
| Dosis | dose | TEXT |
| Estado del dato | data_status | ENUM('Confirmado','Parcial','Pendiente','Cancelado','No registrado') |
| Fuente | source | TEXT |
| Notas internas | internal_notes | TEXT |

### Tabla `users`

| Campo | Tipo |
|-------|------|
| user_id | INT PK AUTO_INCREMENT |
| username | VARCHAR(100) UNIQUE |
| password_hash | VARCHAR(255) |
| role | ENUM('viewer','regulatory','admin') |
| active | TINYINT(1) DEFAULT 1 |
| created_at | TIMESTAMP |

---

## 4. API REST — endpoints requeridos

| Método | Endpoint | Descripción |
|--------|---------|-------------|
| GET | `/api/products` | Lista de productos. Params: `country`, `organism`, `formulation`, `titular`, `status`, `tier` |
| GET | `/api/products/{id}` | Detalle de un producto (todas sus filas país) |
| GET | `/api/export` | Exportación. Params: `format` (pdf_table / pdf_ficha / xlsx), `lang` (es/en/ca), + filtros de `/api/products` |
| GET | `/api/stats` | KPIs para el dashboard |
| POST | `/api/auth/login` | Autenticación. Body: `{username, password}` |
| POST | `/api/auth/logout` | Cierre de sesión |
| GET | `/api/admin/users` | Lista de usuarios (solo admin) |
| POST | `/api/admin/users` | Crear usuario (solo admin) |
| PATCH | `/api/admin/users/{id}` | Actualizar usuario (solo admin) |

---

## 5. Seguridad

- Todas las queries usan **prepared statements** (PDO o MySQLi).
- Las contraseñas se almacenan con `password_hash()` de PHP (bcrypt).
- Cada endpoint de la API verifica la sesión activa y el rol antes de procesar.
- El directorio `/data/` no debe ser accesible desde la web (añadir `.htaccess` de denegación).
- Los archivos de exportación generados se sirven como descarga directa y no se almacenan en disco.

---

## 6. Referencia visual

Ver `/docs/EXPORT_REFERENCE.png` para el formato de la tabla comparativa exportada.

**Lo que SÍ se aplica de la referencia:**
- Estructura de columnas por producto y filas por parámetro.
- Estilo visual limpio, apto para presentación o envío.
- Cabecera con nombre del producto destacado.

**Lo que NO se implementa:**
- Sistema de clasificación por estrellas.
- Puntuación numérica total.

---

## 7. Entregables mínimos por caso

1. `data/schema_mysql.sql` — schema MySQL ejecutable.
2. `data/seed_mysql.sql` — seed con los 44 registros del CSV.
3. `backend/` — API PHP con todos los endpoints de la sección 4.
4. `frontend/` — Interfaz web con las 6 pantallas de la sección 2.
5. `INSTALL.md` — Instrucciones de despliegue en Plesk (paso a paso).
6. `RESULTS_X.md` — Log de decisiones de diseño y observaciones.

---

*Biocontrol Technologies · Documento interno · v1.0 · Mayo 2026*
