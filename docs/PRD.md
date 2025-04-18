# Documento de Especificación de Requisitos (PRD) - Sistema LTI-ATS

## 1. Resumen Ejecutivo

El Sistema de Seguimiento de Talento (LTI-ATS) es una plataforma empresarial diseñada para gestionar el ciclo completo de reclutamiento y selección, implementando principios de Domain-Driven Design (DDD) y arquitectura limpia.

## 2. Visión del Producto

### 2.1 Objetivo Principal
Crear una plataforma robusta y escalable que permita gestionar eficientemente el proceso completo de reclutamiento, desde la publicación de vacantes hasta la contratación final.

### 2.2 Propuesta de Valor
- Gestión centralizada de procesos de reclutamiento
- Seguimiento detallado de candidatos
- Flujos de entrevista personalizables
- Arquitectura escalable y mantenible
- Interfaz intuitiva y moderna

## 3. Arquitectura del Sistema

### 3.1 Stack Tecnológico
- **Frontend**: React 18 + TypeScript + Material-UI
- **Backend**: Node.js + Express + TypeScript
- **Base de Datos**: PostgreSQL 14+
- **ORM**: Prisma
- **Contenedorización**: Docker
- **Testing**: Jest + React Testing Library

### 3.2 Arquitectura de Dominio (DDD)
```
src/
├── domain/         # Entidades y reglas de negocio
├── application/    # Casos de uso y servicios
├── infrastructure/ # Implementaciones técnicas
└── presentation/   # Controladores y API
```

## 4. Modelo de Datos Mejorado

### 4.1 Entidades Principales

1. **Company**
   - Gestión de empresas y sus perfiles
   - Relaciones con empleados y posiciones
   - Campos de auditoría y seguimiento

2. **Position**
   - Gestión de vacantes
   - Flujos de entrevista personalizados
   - Detalles completos del puesto
   - Estados y visibilidad

3. **InterviewFlow**
   - Flujos personalizables por posición
   - Pasos secuenciales
   - Tipos de entrevista configurables

4. **Candidate**
   - Información personal y profesional
   - Historial académico y laboral
   - Gestión de documentos

5. **Application**
   - Seguimiento de postulaciones
   - Estados del proceso
   - Notas y retroalimentación

6. **Interview**
   - Programación de entrevistas
   - Evaluaciones y resultados
   - Seguimiento del proceso

### 4.2 Optimizaciones Implementadas

#### Índices Estratégicos
- Búsqueda por email (Candidate, Employee)
- Filtrado por estado (Application, Position)
- Búsqueda por nombre (Company, Position)
- Ordenamiento por fecha (Interview, Application)

#### Campos de Auditoría
- `createdAt`, `updatedAt` en todas las tablas
- Seguimiento de cambios y versiones
- Historial de modificaciones

## 5. Requisitos Funcionales

### 5.1 Gestión de Empresas
- **RF1**: CRUD de perfiles empresariales
- **RF2**: Gestión de empleados y roles
- **RF3**: Configuración de flujos de entrevista

### 5.2 Gestión de Posiciones
- **RF4**: Publicación y gestión de vacantes
- **RF5**: Configuración de requisitos
- **RF6**: Asignación de flujos de entrevista

### 5.3 Gestión de Candidatos
- **RF7**: Registro y actualización de perfiles
- **RF8**: Gestión de documentos
- **RF9**: Seguimiento de aplicaciones

### 5.4 Proceso de Entrevistas
- **RF10**: Programación de entrevistas
- **RF11**: Registro de evaluaciones
- **RF12**: Seguimiento del proceso

## 6. Requisitos No Funcionales

### 6.1 Rendimiento
- Tiempo de respuesta < 2 segundos
- Soporte para múltiples usuarios concurrentes
- Optimización de consultas a base de datos

### 6.2 Seguridad
- Autenticación y autorización robusta
- Protección de datos sensibles
- Validación de entradas
- Sanitización de archivos

### 6.3 Escalabilidad
- Arquitectura modular
- Base de datos optimizada
- Caché estratégico
- Microservicios preparados

## 7. API REST

### 7.1 Endpoints Principales
```
POST   /api/companies
POST   /api/positions
POST   /api/candidates
POST   /api/applications
POST   /api/interviews
```

### 7.2 Documentación
- OpenAPI 3.0
- Swagger UI integrada
- Ejemplos de uso

## 8. Criterios de Aceptación

### 8.1 Funcionales
- Flujo completo de reclutamiento operativo
- Gestión efectiva de entrevistas
- Sistema de evaluación funcional

### 8.2 Técnicos
- Cobertura de pruebas > 80%
- Documentación actualizada
- Código limpio y mantenible

## 9. Roadmap de Implementación

### Fase 1 - Core (Completado)
1. ✅ Configuración de infraestructura
2. ✅ Implementación de modelos base
3. ✅ Desarrollo de API REST
4. ✅ Interfaz básica

### Fase 2 - Mejoras (En Progreso)
1. 🔄 Sistema de búsqueda avanzada
2. 🔄 Filtros y reportes
3. 🔄 Notificaciones
4. 🔄 Integración con servicios externos

## 10. Métricas de Éxito
- Tiempo promedio de contratación reducido en 30%
- Satisfacción del usuario > 4.5/5
- Disponibilidad del sistema > 99.9%

---
Actualizado: 17/04/2024
Por: ACBG