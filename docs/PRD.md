# Documento de Especificación de Requisitos (PRD) - Sistema de Seguimiento de Talento

## 1. Resumen Ejecutivo
El Sistema de Seguimiento de Talento es una aplicación full-stack diseñada para gestionar y hacer seguimiento de candidatos en procesos de reclutamiento, implementando principios de Domain-Driven Design (DDD) y arquitectura limpia.

## 2. Visión del Producto

### 2.1 Objetivo Principal

Crear una plataforma robusta y escalable que permita gestionar eficientemente el proceso de seguimiento de candidatos, desde su registro inicial hasta la gestión de su información profesional y académica.

### 2.2 Propuesta de Valor

- Centralización de información de candidatos
- Gestión estructurada de perfiles profesionales
- Interfaz intuitiva y moderna
- Arquitectura escalable y mantenible

## 3. Arquitectura del Sistema

### 3.1 Stack Tecnológico

- Frontend: React + TypeScript
- Backend: Node.js + Express + TypeScript
- Base de Datos: PostgreSQL
- ORM: Prisma
- Contenedorización: Docker

### 3.2 Arquitectura de Dominio (DDD)

La aplicación está estructurada siguiendo los principios de DDD:
```markdown
backend/
├── src/
│   ├── domain/         # Entidades y reglas de negocio
│   ├── application/    # Casos de uso y servicios de aplicación
│   ├── infrastructure/ # Implementaciones técnicas
│   └── presentation/   # Controladores y rutas API
```

## 4. Modelo de Datos

### 4.1 Entidades Principales

1. Candidate
	- Información personal básica
	- Relaciones one-to-many con Education, WorkExperience y Resume
2. Education
	- Historial académico
	- Vinculación con Candidate
3. WorkExperience
	- Experiencia laboral
	- Vinculación con Candidate
4. Resume
	- Gestión de documentos CV
	- Vinculación con Candidate

### 5. Requisitos Funcionales MVP

### 5.1 Gestión de Candidatos

- RF1: Registro de nuevos candidatos
- RF2: Validación de datos personales
- RF3: Gestión de información académica
- RF4: Gestión de experiencia laboral
- RF5: Carga y gestión de CV

### 5.2 API REST

- Endpoints Principales
```markdown
  POST /candidates    # Crear nuevo candidato
  POST /upload       # Subir documentos CV
```

### 5.3 Validaciones

- Validación de formato de email
- Validación de número telefónico
- Restricciones de longitud en campos
- Validación de tipos de archivo (PDF/DOCX)

## 6. Requisitos No Funcionales

### 6.1 Rendimiento

- Tiempo de respuesta < 2 segundos
- Soporte para múltiples usuarios concurrentes

### 6.2 Seguridad

- Validación de datos de entrada
- Sanitización de archivos subidos
- Protección contra inyección SQL (via Prisma)

### 6.3 Escalabilidad

- Arquitectura containerizada
- Base de datos relacional optimizada
- Diseño modular para futuras extensiones

## 7. Restricciones Técnicas

### 7.1 Base de Datos

- PostgreSQL como RDBMS principal
- Esquema Prisma con relaciones definidas
- Índices optimizados para búsquedas frecuentes

### 7.2 Almacenamiento

- Soporte para archivos PDF y DOCX
- Gestión de rutas de archivo seguras
- Validación de tipos MIME

## 8. Criterios de Aceptación

### 8.1 Funcionales

- Registro exitoso de candidatos con datos completos
- Carga y recuperación correcta de CV
- Validación apropiada de todos los campos

### 8.2 Técnicos

- Tests unitarios pasando
- Integración continua funcionando
- Documentación API completa (OpenAPI)

## 9. Roadmap de Desarrollo

### Fase 1 - MVP

1. Configuración de infraestructura
2. Implementación de modelos base
3. Desarrollo de API REST
4. Desarrollo de interfaz de usuario básica

### Fase 2 - Mejoras

- Implementación de búsqueda avanzada
- Sistema de filtros
- Reportes y análisis
- Mejoras en la experiencia de usuario

## 10. Métricas de Éxito

- Tiempo de registro de candidatos < 5 minutos
- Tasa de error en carga de documentos < 1%
- Disponibilidad del sistema > 99.9%

**Este PRD** establece las bases para el desarrollo del **MVP del Sistema (LTI) de Seguimiento de Talento (ATS), siguiendo las mejores prácticas de DDD y arquitectura limpia.
 
---

## ACTUALIZADO: 17/04/2025
### ACBG

---