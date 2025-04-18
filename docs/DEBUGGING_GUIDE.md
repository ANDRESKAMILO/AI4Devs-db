# Curso: Debugging y Solución de Problemas en Sistemas ATS con PostgreSQL y Prisma

## Índice
1. [Introducción](#introducción)
2. [Herramientas de Diagnóstico](#herramientas-de-diagnóstico)
3. [Problemas Comunes](#problemas-comunes)
4. [Soluciones Implementadas](#soluciones-implementadas)
5. [Estrategias Avanzadas de Debugging](#estrategias-avanzadas-de-debugging)
6. [Tips y Consejos](#tips-y-consejos)
7. [Ejercicios Prácticos](#ejercicios-prácticos)
8. [Recursos Adicionales](#recursos-adicionales)

## Introducción

El debugging es una habilidad esencial en el desarrollo de aplicaciones con bases de datos. Este curso proporciona una guía sistemática para identificar, diagnosticar y resolver problemas comunes en sistemas ATS que utilizan PostgreSQL y Prisma ORM.

### Objetivos de Aprendizaje
- Comprender el flujo de errores entre la aplicación, Prisma y PostgreSQL
- Dominar técnicas de diagnóstico para problemas de base de datos
- Implementar soluciones eficientes a problemas frecuentes
- Aplicar metodologías preventivas para minimizar errores

## Herramientas de Diagnóstico

### PgAdmin
- **Uso**: Verificación visual de la estructura de la base de datos
- **Diagnóstico**: `Tools > Query Tool` para ejecutar consultas de diagnóstico
- **Visualización**: `ERD Tool` para verificar relaciones

### Registros (Logs)
- **PostgreSQL logs**: 
```bash
# Ubicación en Windows
C:\Program Files\PostgreSQL\14\data\log\
```
- **Prisma logs**: 
```bash
npx prisma migrate dev --preview-feature --verbose
```

### Herramientas CLI
- **psql**: Cliente nativo de PostgreSQL
```bash
psql -U postgres -d ats_database
```
- **Prisma CLI**: Herramientas de diagnóstico
```bash
npx prisma format
npx prisma validate
```

## Problemas Comunes

### 1. Conexión a PostgreSQL
**Problema**: Error de autenticación
```bash
psql: error: FATAL:  password authentication failed
```

**Diagnóstico**:
1. Verificar configuración en `pg_hba.conf`
2. Comprobar credenciales en variables de entorno

**Solución**:
1. Verificar credenciales en `.env`
2. Resetear contraseña:
```sql
ALTER USER postgres WITH PASSWORD 'MiPostgre2025!DB';
```

### 2. Migraciones de Prisma
**Problema**: Error de conexión
```
Error: P1001: Can't reach database server
```

**Diagnóstico**:
1. Verificar estado del servicio PostgreSQL
2. Comprobar parámetros de conexión
3. Validar permisos de usuario

**Solución**:
1. Verificar servicio PostgreSQL:
```bash
# Windows
services.msc  # Buscar PostgreSQL
```
2. Verificar URL en .env
3. Limpiar migraciones:
```bash
rm -rf prisma/migrations/
npx prisma migrate dev --name complete_ats_schema
```

### 3. Conflictos de Esquema
**Problema**: Inconsistencias entre modelo Prisma y base de datos

**Diagnóstico**:
```bash
npx prisma db pull --print
# Comparar con schema.prisma
```

**Solución**:
```bash
# Actualizar base de datos para coincidir con el esquema:
npx prisma db push

# Alternativa: actualizar el esquema para coincidir con la base de datos:
npx prisma db pull
```

## Estrategias Avanzadas de Debugging

### Análisis de Rendimiento
- **Identificar consultas lentas**:
```sql
SELECT * FROM pg_stat_activity WHERE state = 'active';
```

- **Análisis de plan de ejecución**:
```sql
EXPLAIN ANALYZE SELECT * FROM candidatos WHERE skill_id = 1;
```

### Troubleshooting de Índices
- **Verificar índices existentes**:
```sql
SELECT * FROM pg_indexes WHERE tablename = 'candidatos';
```

- **Crear índices para mejorar rendimiento**:
```sql
CREATE INDEX idx_candidatos_skill ON candidatos(skill_id);
```

## Tips y Consejos

### 1. Desarrollo Iterativo
- Comenzar con un esquema simple
- Agregar complejidad gradualmente
- Probar cada cambio antes de proceder
- **Estrategia recomendada**: Implementar un "feature flag" para nuevas características

### 2. Gestión de Versiones
- Crear ramas para cambios mayores
- Documentar cambios en commits
- Mantener backups de la base de datos
- **Consejo**: Automatizar backups antes de migraciones importantes

### 3. Debugging Efectivo
- Usar logs de PostgreSQL
- Verificar syntax en schema.prisma
- Mantener documentación actualizada
- **Técnica avanzada**: Implementar "canary deployments" para cambios críticos

## Ejercicios Prácticos

### Ejercicio 1: Simulación de Error
1. Introducir un error deliberado en el schema.prisma
2. Diagnósticar usando las herramientas aprendidas
3. Aplicar solución y verificar funcionamiento

### Ejercicio 2: Optimización de Consulta
1. Identificar una consulta que puede mejorar con índices
2. Medir rendimiento antes y después de optimizar
3. Documentar mejoras en rendimiento

## Recursos Adicionales

### Documentación Oficial
- [Documentación de PostgreSQL](https://www.postgresql.org/docs/)
- [Documentación de Prisma](https://www.prisma.io/docs/)

### Herramientas Complementarias
- [pgBadger](https://github.com/darold/pgbadger) - Analizador de logs de PostgreSQL
- [Prisma Studio](https://www.prisma.io/studio) - Interfaz visual para datos
- [pgAdmin](https://www.pgadmin.org/) - Herramienta de administración para PostgreSQL

### Comunidad y Soporte
- [Stack Overflow - Tag Prisma](https://stackoverflow.com/questions/tagged/prisma)
- [Stack Overflow - Tag PostgreSQL](https://stackoverflow.com/questions/tagged/postgresql)
- [GitHub Issues de Prisma](https://github.com/prisma/prisma/issues)