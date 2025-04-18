# Verificación de Estructura con PgAdmin

## Índice
1. [Introducción](#introducción)
2. [Conexión a PostgreSQL](#conexión-a-postgresql)
3. [Verificación de Tablas](#verificación-de-tablas)
4. [Verificación de Índices](#verificación-de-índices)
5. [Verificación de Relaciones](#verificación-de-relaciones)

## Introducción

Este documento describe el proceso de verificación visual de la estructura de la base de datos usando PgAdmin, una herramienta gráfica para PostgreSQL que permite gestionar y visualizar bases de datos de manera intuitiva.

## Conexión a PostgreSQL

1. **Inicio de PgAdmin**:
   - Abrimos PgAdmin desde el menú inicio
   - Nos presenta la interfaz principal con el panel de navegación a la izquierda

2. **Configuración de Conexión**:
   ```
   Nombre: LTI-ATS
   Host: localhost
   Puerto: 5432
   Base de datos de mantenimiento: postgres
   Usuario: postgres
   Contraseña: ********** (ingresada al conectar)
   ```

3. **Verificación de Conexión Exitosa**:
   - Al establecerse la conexión, PgAdmin muestra el árbol de navegación con los servidores disponibles
   - Podemos ver `ai4devs_db` en la lista de bases de datos

## Verificación de Tablas

Navegamos a `Servers > PostgreSQL > Databases > ai4devs_db > Schemas > public > Tables` y verificamos:

1. **Tablas del Sistema**:
   - **Company**: ✅ Presente con todos los campos
   - **Employee**: ✅ Presente con todos los campos
   - **Position**: ✅ Presente con todos los campos
   - **InterviewFlow**: ✅ Presente con todos los campos
   - **InterviewStep**: ✅ Presente con todos los campos
   - **InterviewType**: ✅ Presente con todos los campos
   - **Candidate**: ✅ Presente con todos los campos
   - **Education**: ✅ Presente con todos los campos
   - **WorkExperience**: ✅ Presente con todos los campos
   - **Resume**: ✅ Presente con todos los campos
   - **Application**: ✅ Presente con todos los campos
   - **Interview**: ✅ Presente con todos los campos

2. **Verificación de Columnas**:
   - Haciendo clic derecho en cada tabla y seleccionando "Properties"
   - Revisando la pestaña "Columns" para verificar campos, tipos y restricciones
   - Ejemplo para `Company`:
     - `id`: SERIAL (Primary Key)
     - `name`: VARCHAR(100) NOT NULL
     - `description`: TEXT
     - `createdAt`: TIMESTAMP NOT NULL DEFAULT NOW()
     - `updatedAt`: TIMESTAMP NOT NULL

## Verificación de Índices

Navegamos a cada tabla > Indexes para verificar:

1. **Índices de Búsqueda**:
   - **Company**: ✅ `idx_company_name` en columna `name`
   - **Employee**: ✅ `idx_employee_email` en columna `email`
   - **Position**: ✅ `idx_position_status` en columna `status`
   - **Candidate**: ✅ `idx_candidate_email` en columna `email`

2. **Índices de Relación**:
   - **Position**: ✅ `idx_position_company` en columna `companyId`
   - **InterviewStep**: ✅ `idx_interviewstep_flow` en columna `interviewFlowId`
   - **Application**: ✅ `idx_application_position` en columna `positionId`
   - **Interview**: ✅ `idx_interview_application` en columna `applicationId`

3. **Índices Compuestos**:
   - **Candidate**: ✅ `idx_candidate_lastname_firstname` en columnas `lastName, firstName`

## Verificación de Relaciones

Usando la función "ERD" de PgAdmin:

1. **Creación de Diagrama ER**:
   - Hicimos clic derecho en la base de datos > Generate ERD
   - Seleccionamos todas las tablas para incluirlas

2. **Verificación Visual de Relaciones**:
   - **Company** → **Employee**: Relación One-to-Many ✅
   - **Company** → **Position**: Relación One-to-Many ✅
   - **Position** → **InterviewFlow**: Relación Many-to-One ✅
   - **InterviewFlow** → **InterviewStep**: Relación One-to-Many ✅
   - **InterviewStep** → **InterviewType**: Relación Many-to-One ✅
   - **Position** → **Application**: Relación One-to-Many ✅
   - **Candidate** → **Application**: Relación One-to-Many ✅
   - **Application** → **Interview**: Relación One-to-Many ✅
   - **Candidate** → **Education**: Relación One-to-Many ✅
   - **Candidate** → **WorkExperience**: Relación One-to-Many ✅
   - **Candidate** → **Resume**: Relación One-to-Many ✅

3. **Verificación de Cardinalidad**:
   - Relaciones correctamente implementadas con las restricciones apropiadas
   - Claves foráneas configuradas con acciones de actualización y eliminación

## Pruebas de Integridad

1. **Prueba de Integridad Referencial**:
   - Intentamos eliminar un registro Company con Employees asociados
   - Resultado: ✅ Restricción enforced, error de eliminación como se esperaba

2. **Prueba de Unicidad**:
   - Intentamos crear un Employee con un email duplicado
   - Resultado: ✅ Restricción enforced, error de duplicidad como se esperaba

## Conclusión

La verificación visual con PgAdmin confirma que:
- ✅ La estructura de base de datos se implementó correctamente
- ✅ Todos los índices están configurados para optimizar consultas
- ✅ Las relaciones entre entidades son consistentes
- ✅ Las restricciones de integridad funcionan como se esperaba

La herramienta PgAdmin resultó invaluable para verificar visualmente la implementación y confirmar que el diseño cumple con los requisitos del sistema LTI-ATS.

---
Actualizado: 17/04/2024
Por: ACBG 