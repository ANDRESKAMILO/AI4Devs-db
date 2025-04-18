# Curso: Implementación de Base de Datos Escalable para Sistema ATS

## Índice
1. [Introducción](#introducción)
2. [Requisitos Previos](#requisitos-previos)
3. [Fase 1: Análisis y Diseño](#fase-1-análisis-y-diseño)
4. [Fase 2: Configuración del Entorno](#fase-2-configuración-del-entorno)
5. [Fase 3: Implementación del Esquema](#fase-3-implementación-del-esquema)
6. [Fase 4: Migraciones y Despliegue](#fase-4-migraciones-y-despliegue)
7. [Fase 5: Verificación y Pruebas](#fase-5-verificación-y-pruebas)
8. [Fase 6: Documentación Final](#fase-6-documentación-final)
9. [Problemas Comunes y Soluciones](#problemas-comunes-y-soluciones)
10. [Lecciones Aprendidas](#lecciones-aprendidas)
11. [Recursos Adicionales](#recursos-adicionales)

## Introducción

Este curso documenta el proceso detallado para implementar y mejorar una base de datos escalable para un Sistema de Seguimiento de Talento (ATS). El enfoque se centra en:

- Aplicar principios de Domain-Driven Design (DDD)
- Implementar buenas prácticas de diseño de bases de datos
- Optimizar el rendimiento mediante índices estratégicos
- Usar herramientas modernas como Prisma y PostgreSQL

El resultado final es una base de datos robusta y optimizada que puede manejar el flujo completo de reclutamiento, desde la publicación de posiciones hasta la contratación de candidatos.

## Requisitos Previos

### Conocimientos Técnicos
- Fundamentos de bases de datos relacionales
- Conocimiento básico de SQL
- Experiencia con Node.js y npm
- Familiaridad con TypeScript (recomendado)

### Herramientas Necesarias
```bash
# Verificar Node.js y npm
node -v  # Debe ser v16.x o superior
npm -v   # Debe ser v8.x o superior

# Verificar PostgreSQL
psql --version  # Debe ser v14.x o superior

# IDE recomendado: Visual Studio Code
code --version
```

### Configuración Inicial del Proyecto
```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/LTI-ATS.git
cd LTI-ATS

# Instalar dependencias
cd backend
npm install

# Verificar estructura inicial
ls -la
```

## Fase 1: Análisis y Diseño

### 1.1 Análisis de Requisitos
En esta etapa, analizamos los requisitos del sistema ATS:
- Gestión de empresas y empleados
- Publicación y seguimiento de posiciones
- Flujos de entrevista personalizables
- Gestión de candidatos y aplicaciones
- Evaluación y seguimiento de entrevistas

### 1.2 Modelado de Datos
Basado en el análisis, identificamos las siguientes entidades principales:
- Company (Empresa)
- Employee (Empleado)
- Position (Posición)
- InterviewFlow (Flujo de entrevista)
- InterviewStep (Paso de entrevista)
- InterviewType (Tipo de entrevista)
- Candidate (Candidato)
- Education (Educación)
- WorkExperience (Experiencia laboral)
- Resume (CV)
- Application (Aplicación)
- Interview (Entrevista)

### 1.3 Diseño del ERD
Creamos un diagrama entidad-relación (ERD) para visualizar las relaciones:
```mermaid
erDiagram
    Company ||--o{ Employee : employs
    Company ||--o{ Position : offers
    Position ||--|| InterviewFlow : "has flow"
    InterviewFlow ||--o{ InterviewStep : contains
    InterviewStep ||--|| InterviewType : "has type"
    Position ||--o{ Application : receives
    Candidate ||--o{ Application : submits
    Application ||--o{ Interview : schedules
    Interview ||--|| InterviewStep : "follows step"
    Employee ||--o{ Interview : conducts
    Candidate ||--o{ Education : "has education"
    Candidate ||--o{ WorkExperience : "has experience"
    Candidate ||--o{ Resume : "has resume"
```

### 1.4 Control de Versiones
Configuramos una rama específica para el desarrollo de la base de datos:
```bash
# Crear y cambiar a una nueva rama
git checkout -b db-ACBG

# Crear estructura de documentación
mkdir -p docs
```

## Fase 2: Configuración del Entorno

### 2.1 Instalación de PostgreSQL
Si no tienes PostgreSQL instalado:
```bash
# En Windows, descarga el instalador de postgresql.org
# En Linux (Ubuntu/Debian)
sudo apt update
sudo apt install postgresql postgresql-contrib

# Iniciar el servicio
sudo service postgresql start
```

### 2.2 Creación de Base de Datos
```bash
# Conectar a PostgreSQL
psql -U postgres

# Crear base de datos
CREATE DATABASE ai4devs_db;

# Verificar creación
\l
```

### 2.3 Configuración de Variables de Entorno
```bash
# Crear archivo .env (¡IMPORTANTE: Nunca subir a Git!)
echo "DATABASE_URL=\"postgresql://postgres:TU_CONTRASEÑA@localhost:5432/ai4devs_db\"" > backend/.env

# Agregar .env a .gitignore
echo ".env" >> .gitignore
```

### 2.4 Inicialización de Prisma ORM
```bash
# Entrar al directorio backend
cd backend

# Inicializar Prisma
npx prisma init

# Verificar archivos generados
ls -la prisma/
```

## Fase 3: Implementación del Esquema

### 3.1 Definición del Esquema Prisma
Editamos `prisma/schema.prisma` para definir nuestras entidades:

```prisma
// Configuración del generador de cliente
generator client {
  provider      = "prisma-client-js"
  binaryTargets = ["native", "debian-openssl-3.0.x"]
}

// Configuración de la fuente de datos
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// Definición de modelos
model Company {
  id          Int         @id @default(autoincrement())
  name        String      @db.VarChar(100)
  description String?     @db.Text
  createdAt   DateTime    @default(now())
  updatedAt   DateTime    @updatedAt
  
  // Relaciones
  employees   Employee[]
  positions   Position[]

  // Índices
  @@index([name])
}

// ... más modelos
```

### 3.2 Implementación de Índices Estratégicos
Para optimizar consultas frecuentes, agregamos índices:

```prisma
model Candidate {
  // ... campos
  
  // Índices para búsquedas frecuentes
  @@index([email])
  @@index([lastName, firstName])
}
```

### 3.3 Definición de Relaciones
Definimos relaciones entre entidades con restricciones apropiadas:

```prisma
model Position {
  // ... campos
  
  // Relaciones
  company        Company        @relation(fields: [companyId], references: [id])
  interviewFlow  InterviewFlow  @relation(fields: [interviewFlowId], references: [id])
  applications   Application[]
}
```

## Fase 4: Migraciones y Despliegue

### 4.1 Creación de Migraciones
Generamos las migraciones de la base de datos:

```bash
# Generar migración
npx prisma migrate dev --name complete_ats_schema

# Verificar archivos generados
ls -la prisma/migrations/
```

### 4.2 Aplicación de Migraciones
Aplicamos las migraciones a la base de datos:

```bash
# Aplicar migración en desarrollo
npx prisma migrate dev

# Para entornos de producción
# npx prisma migrate deploy
```

### 4.3 Generación del Cliente Prisma
Generamos el cliente Prisma para interactuar con la base de datos:

```bash
# Generar cliente
npx prisma generate

# Verificar cliente generado
ls -la node_modules/.prisma/client/
```

## Fase 5: Verificación y Pruebas

### 5.1 Verificación en PostgreSQL
Verificamos la estructura creada directamente en PostgreSQL:

```sql
-- Conectar a la base de datos
psql -U postgres -d ai4devs_db

-- Listar tablas
\dt

-- Verificar índices
\di

-- Examinar estructura de tabla específica
\d+ "Company"
```

### 5.2 Verificación Visual con PgAdmin
1. Abrimos PgAdmin y conectamos a la base de datos
2. Navegamos a `Servers > PostgreSQL > Databases > ai4devs_db > Schemas > public > Tables`
3. Verificamos:
   - Estructura de tablas y columnas
   - Índices y restricciones
   - Relaciones entre entidades

### 5.3 Pruebas de Integridad
Verificamos que las restricciones funcionen correctamente:

```sql
-- Prueba de integridad referencial
-- Intentar eliminar una empresa con empleados (debe fallar)
DELETE FROM "Company" WHERE id = 1;

-- Prueba de unicidad
-- Intentar crear un empleado con email duplicado (debe fallar)
INSERT INTO "Employee" (companyId, email, name, role, isActive)
VALUES (1, 'correo_existente@ejemplo.com', 'Duplicado', 'Rol', true);
```

## Fase 6: Documentación Final

### 6.1 Organización de Documentación

Hemos organizado toda la documentación en la carpeta `/docs` con el siguiente esquema:

```bash
docs/
├── README.md                # Documentación principal con índice
├── PRD.md                   # Documento de requisitos del producto
├── DEBUGGING_GUIDE.md       # Guía de solución de problemas
├── SUMMARY_CURSO.md         # Este documento detallado
├── PROCESO_IMPLEMENTACION.md # Proceso paso a paso
├── PGADMIN_VERIFICACION.md  # Verificación con pgAdmin
├── DB_UPGRADE_GUIDE.md      # Guía de actualización de BD
├── ERD.md                   # Diagrama ER original
└── ERD_ACTUALIZADO.md       # Diagrama ER actualizado
```

### 6.2 Creación de Índice de Documentación

Hemos agregado un índice de documentación completo en el archivo `README.md` principal de la carpeta `/docs`, que incluye:

- **Descripción de cada documento**: Breve resumen de qué contiene cada archivo
- **Etiquetas de uso e importancia**: Categorización con etiquetas como #Referencia, #Capacitación, #Soporte, etc.
- **Estructura organizada**: Tabla clara que facilita encontrar la información necesaria

Este enfoque facilita la navegación por toda la documentación del proyecto y permite a nuevos miembros del equipo entender rápidamente dónde encontrar la información que necesitan.

### 6.3 Formato Tipo Curso

Todos los documentos principales siguen un formato tipo "CURSO" con:

- Índice navegable con enlaces de anclaje
- Estructura progresiva desde conceptos básicos hasta avanzados
- Ejemplos prácticos y código listo para usar
- Secciones de problemas comunes y soluciones
- Lecciones aprendidas y mejores prácticas

## Problemas Comunes y Soluciones

### Error de Conexión a PostgreSQL
```
Error: P1001: Can't reach database server
```

**Solución**:
1. Verificar que PostgreSQL esté ejecutándose
2. Comprobar credenciales en `.env`
3. Verificar firewall y permisos

### Error en Migraciones
```
Error: P1001: Migration `XXXX` failed to apply cleanly
```

**Solución**:
1. Revisar logs para identificar el error específico
2. En entorno de desarrollo, resetear la base de datos:
   ```bash
   npx prisma migrate reset
   ```
3. Corregir el esquema y generar una nueva migración

### Errores de Sintaxis en Prisma
```
Error: P1012: Prisma schema validation
```

**Solución**:
1. Revisar la línea indicada en el error
2. Verificar sintaxis correcta en la documentación de Prisma
3. Utilizar extensiones de VSCode para validación en tiempo real

## Lecciones Aprendidas

### Diseño y Modelado
- **Planificación**: Invertir tiempo en el diseño previo ahorra horas de depuración
- **Normalización**: Aplicar correctamente la normalización evita redundancias y mejora integridad
- **Entidades**: Identificar claramente las entidades y sus relaciones antes de implementar

### Optimización
- **Índices**: Crear índices estratégicamente en campos de búsqueda frecuente
- **Tipos de datos**: Seleccionar tipos de datos apropiados según el contenido
- **Restricciones**: Implementar restricciones para garantizar la integridad de los datos

### Seguridad
- **Variables de entorno**: Nunca incluir credenciales en el código fuente
- **Protección**: Utilizar .gitignore para evitar exponer información sensible
- **Auditoría**: Implementar campos de auditoría (createdAt, updatedAt) en todas las tablas

### Herramientas
- **Prisma**: ORM potente que simplifica interacciones con la base de datos
- **PgAdmin**: Herramienta visual invaluable para verificación y depuración
- **Control de versiones**: Crear ramas específicas para cambios en la base de datos

## Recursos Adicionales

### Documentación
- [Documentación oficial de Prisma](https://www.prisma.io/docs/)
- [Documentación de PostgreSQL](https://www.postgresql.org/docs/)
- [Guía de PgAdmin](https://www.pgadmin.org/docs/)

### Tutoriales Recomendados
- [Modelado de datos con Prisma](https://www.prisma.io/docs/concepts/components/prisma-schema)
- [Migraciones en Prisma](https://www.prisma.io/docs/concepts/components/prisma-migrate)
- [Optimización de bases de datos PostgreSQL](https://www.postgresql.org/docs/current/performance-tips.html)

### Herramientas Complementarias
- [DBeaver](https://dbeaver.io/) - Cliente de base de datos alternativo
- [DbDiagram](https://dbdiagram.io/) - Herramienta para diseño visual de bases de datos
- [Prisma Studio](https://www.prisma.io/studio) - UI para explorar y manipular datos

---

## Autor
ACBG - Abril 2024

## Licencia
Este material está disponible bajo la licencia MIT. 