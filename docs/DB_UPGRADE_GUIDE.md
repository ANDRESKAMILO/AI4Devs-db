# Guía de Actualización de Base de Datos LTI-ATS

## Índice
1. [Preparación](#preparación)
2. [Nuevas Entidades](#nuevas-entidades)
3. [Proceso de Migración](#proceso-de-migración)
4. [Verificación](#verificación)
5. [Rollback](#rollback)

## Preparación

### 1. Respaldo de Datos
```bash
# Backup de la base de datos actual
pg_dump -U postgres -d ai4devs_db > backup_$(date +%Y%m%d).sql
```

### 2. Verificación de Ambiente
```bash
# Verificar conexión
psql -U postgres -d ai4devs_db -c "\conninfo"

# Verificar espacio disponible
psql -U postgres -d ai4devs_db -c "\l+"
```

## Nuevas Entidades

### 1. Estructura de Tablas

#### Company
```sql
CREATE TABLE "Company" (
    "id" SERIAL PRIMARY KEY,
    "name" VARCHAR(100) NOT NULL,
    "description" TEXT,
    "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" TIMESTAMP NOT NULL
);

-- Índices
CREATE INDEX "idx_company_name" ON "Company"("name");
```

#### Position
```sql
CREATE TABLE "Position" (
    "id" SERIAL PRIMARY KEY,
    "companyId" INTEGER NOT NULL,
    "title" VARCHAR(100) NOT NULL,
    -- ... otros campos
    FOREIGN KEY ("companyId") REFERENCES "Company"("id")
);

-- Índices
CREATE INDEX "idx_position_company" ON "Position"("companyId");
CREATE INDEX "idx_position_status" ON "Position"("status");
```

### 2. Tipos Enumerados
```sql
CREATE TYPE "employment_type" AS ENUM (
    'FULL_TIME',
    'PART_TIME',
    'CONTRACT',
    'TEMPORARY',
    'INTERNSHIP'
);

CREATE TYPE "application_status" AS ENUM (
    'PENDING',
    'REVIEWING',
    'INTERVIEWING',
    'REJECTED',
    'ACCEPTED'
);
```

## Proceso de Migración

### 1. Generar Migración
```bash
# Crear migración Prisma
npx prisma migrate dev --name complete_ats_schema

# Verificar archivos generados
ls -la prisma/migrations/
```

### 2. Aplicar Migración
```bash
# Ambiente de desarrollo
npx prisma migrate dev

# Ambiente de producción
npx prisma migrate deploy
```

### 3. Verificar Estructura
```sql
-- Listar tablas
\dt

-- Verificar índices
\di

-- Verificar relaciones
\d+ "Position"
```

## Verificación

### 1. Pruebas de Integridad
```sql
-- Verificar foreign keys
SELECT
    tc.table_schema, 
    tc.constraint_name, 
    tc.table_name, 
    kcu.column_name,
    ccu.table_name AS foreign_table_name,
    ccu.column_name AS foreign_column_name 
FROM 
    information_schema.table_constraints AS tc 
    JOIN information_schema.key_column_usage AS kcu
      ON tc.constraint_name = kcu.constraint_name
    JOIN information_schema.constraint_column_usage AS ccu
      ON ccu.constraint_name = tc.constraint_name
WHERE constraint_type = 'FOREIGN KEY';
```

### 2. Pruebas de Rendimiento
```sql
-- Analizar consultas comunes
EXPLAIN ANALYZE
SELECT p.* 
FROM "Position" p
JOIN "Company" c ON p.company_id = c.id
WHERE p.status = 'ACTIVE';
```

## Rollback

### 1. Plan de Rollback
```bash
# Revertir última migración
npx prisma migrate reset

# Restaurar backup si necesario
psql -U postgres -d ai4devs_db < backup_[fecha].sql
```

### 2. Verificación Post-Rollback
```sql
-- Verificar estado de tablas
\dt
\d+ "Position"
```

## Notas Importantes

1. **Índices Estratégicos**:
   - Búsqueda por email: mejora consultas de autenticación
   - Filtrado por estado: optimiza listados
   - Búsqueda por nombre: acelera búsquedas textuales

2. **Campos de Auditoría**:
   - `createdAt`: Registro de creación
   - `updatedAt`: Última modificación
   - Triggers automáticos para `updatedAt`

3. **Optimizaciones**:
   - Particionamiento por fecha en tablas grandes
   - Índices parciales para estados activos
   - Índices compuestos para consultas frecuentes

---
Actualizado: 17/04/2024
Por: ACBG 