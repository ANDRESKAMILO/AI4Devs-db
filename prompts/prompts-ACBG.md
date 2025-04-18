# Prompts Utilizados para la Actualización de Base de Datos LTI-ATS

## Implementación de Estructura Completa y Normalizada

### Prompt 1: Análisis inicial y creación del PRD -- Cursor/ ASK-Auto Select
#### Contexto: README.md - frontend/ - backend/
```
Por favor actúa como un Arquitecto de Sistemas y DBA experto en el diseño de sistemas escalables usando DDD y analiza este Proyecto.

1. Requiero que lo estudies a fondo y lo asimiles en su totalidad.
2. Que documentes por favor el Documento de Especificación de Requisitos para el MVP del Sistema (PRD).
```

### Prompt 2: Actualización de base de datos -- Cursor/ Agent-Auto Select
#### Contexto: README.md @PRD.md @ERD.md frontend/ backend/
```
- Crear y/o revisar la estructura de BASE DE DATOS.
- Actualizar la base de datos con las nuevas entidades que nos permitan operar el flujo completo de aplicación para diversas posiciones. 
- Verificar que se estén aplicando las buenas prácticas, como la definición de índices y la normalización de la base datos.
- Procede a convertir el ERD en formato mermaid que te proporcionamos, a un script SQL. 
- Analiza la base de datos del código actual y el script SQL y expande la estructura de datos usando las migraciones de Prisma.
- Por favor proveernos con el diagrama ERD de esta base de datos mejorada.
- Verificar que se estén aplicando las buenas prácticas, como la definición de índices y la normalización de la base datos.
- Por favor mejorar esta estructura y normalizarla.
- Utiliza herramientas visuales para bases de datos PostgreSQL como PGAdmin para verificar que puedes conectar, y que la estructura creada es correcta.
```

### Prompt 3: Implementación y verificación -- Cursor/ ASK-Auto Select
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/
```
- Ayúdame a crear una rama, hacer los commit pendientes al repositorio.
- Necesito verificar la documentación y archivos nuevos generados.
- Crear una nueva rama para tu entregable con el nombre "db-iniciales" mis iniciales: "ACBG".
```

### Prompt 4: Preparación de entorno PostgreSQL -- Cursor/ Agent-claude-3.5-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/
```
- ¿Tienes acceso a pgAdmin? 
- Verifica los resultados de la base de datos.
- Oye, antes de empezar a realizar cambios, ayúdame a crear una rama, y hacer los commit que estén pendientes por hacer al repositorio.
```

### Prompt 5: Implementación de la migración -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: @README.md @PRD.md @ERD_ACTUALIZADO.md  frontend/  backend/  docs/  @20240417000000_complete_ats_schema.sql
```
- Debo usar este comando "npx prisma migrate dev --name complete_ats_schema" tal cual, o debo cambiar algo?
- Mi archivo dentro de la carpeta manual/ lo nombre tal cual lo sugeriste "backend/prisma/migrations/manual/20240417000000_complete_ats_schema.sql"
```

### Prompt 6: Balance y verificación -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/  @20240417000000_complete_ats_schema.sql
```
Antes que nada quisiera que hagamos una pausa para que hagamos un Balance de lo que llevamos y lo que falta según la misión y las Tareas:

También verificar:
1. Que tengamos en la carpeta "/docs" el documento markdown tipo "CURSO" con el summary de todo el procedimiento realizado de principio a fin y una parte final de lecciones aprendidas.
2. Que tengamos en la carpeta "/docs" el documento markdown tipo "CURSO" con el "Debugging" de todas las dificultades que tuvimos y las descripciones del como lo resolvimos y una parte final de tips y consejos para una próxima vez.
3. Recuerda que la documentación que generes debe contener los paso a paso y comentarios respectivos en las líneas de código para lograr replicarlo un estudiante o un junior.
```

### Prompt 7: Actualización de documentación -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/  @20240417000000_complete_ats_schema.sql
```
- Actualizar el PRD.
- Expandir un poco más del procedimiento de actualización de la base de datos con las nuevas entidades que nos permitan operar el flujo completo de aplicación para diversas posiciones.
```

### Prompt 8: Verificación con PgAdmin -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/  @20240417000000_complete_ats_schema.sql
```
No veo en donde mencionemos que utilizamos herramientas visuales para bases de datos PostgreSQL como PgAdmin para verificar que pudimos conectar, y que la estructura creada es correcta.
```

### Prompt 9: Verificación de Documentación -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/
```
En el README.md de la carpeta "./docs/README.md", podriamos agregar un indice con la documentación complementaria que generamos y que se encuentra en la carpeta "./docs", con una etiqueta de lo que representa para el proyecto, "#uso e importancia".
```

### Prompt 10: Revisión Final del Proyecto -- Cursor/ Agent-claude-3.7-sonnet
#### Contexto: README.md @PRD.md @ERD_ACTUALIZADO.md frontend/ backend/ docs/ @SUMMARY_CURSO.md @DEBUGGING_GUIDE.md
```
1. Hacer una última revisión del proyecto LTI-ATS que cumpla con las buenas practicas DBA, y en el diseño de sistemas escalables usando DDD.
2. Actualizar el archivo de "./prompts/prompts-iniciales.md"
3. Revizar y verificar que el summary este actualizado y de ser necesario actualizar el documento "./docs/SUMMARY_CURSO.md"
4. Listar los comandos necesarios para hacer los commit faltantes.
5. Generarme el PR definido por nuestro CTO director.
```

## Verificación con PgAdmin
Durante el proceso de implementación, utilizamos PgAdmin para verificar visualmente la estructura de la base de datos:

1. **Conexión a la base de datos**:
   - Configuramos PgAdmin para conectar a nuestra instancia local de PostgreSQL
   - Establecimos conexión usando las credenciales configuradas

2. **Verificación de tablas y columnas**:
   - Navegamos por la estructura de la base de datos
   - Verificamos que todas las tablas se crearon correctamente
   - Confirmamos tipos de datos, restricciones y valores por defecto

3. **Verificación de relaciones**:
   - Usamos la función "ERD" para generar un diagrama visual
   - Confirmamos visualmente todas las relaciones entre entidades

4. **Verificación de índices**:
   - Comprobamos la creación correcta de índices en cada tabla
   - Verificamos índices simples, compuestos y únicos

5. **Pruebas de integridad**:
   - Realizamos pruebas para verificar restricciones de integridad
   - Confirmamos el comportamiento correcto de claves foráneas

Este proceso de verificación visual fue documentado detalladamente en el archivo `docs/PGADMIN_VERIFICACION.md`.

## Resultados Obtenidos
A través de estos prompts, se logró:

1. Analizar la estructura existente del sistema
2. Diseñar un esquema de base de datos mejorado y normalizado
3. Implementar índices estratégicos para optimizar consultas
4. Crear migraciones usando Prisma
5. Verificar la estructura en PostgreSQL usando PgAdmin
6. Documentar el proceso completo y mejores prácticas
7. Crear documentación completa en formato de curso para capacitación
8. Actualizar toda la documentación con un formato claro y bien estructurado
9. Organizar la documentación complementaria con etiquetas de uso e importancia

## Observaciones Importantes
- La estructura implementada sigue los principios de Domain-Driven Design
- Se pusieron en práctica los conceptos de normalización de bases de datos
- Se implementaron índices estratégicos para optimizar las consultas más frecuentes
- PgAdmin demostró ser una herramienta invaluable para la verificación visual
- Se documentó todo el proceso para referencia futura 
- La estructura de documentación en formato "CURSO" facilita el aprendizaje y transferencia de conocimiento 