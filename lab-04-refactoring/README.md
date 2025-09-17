# Laboratorio 04: Refactorización con GitHub Copilot

## Prerrequisitos
- Python 3.8+
- IDE con GitHub Copilot instalado (VS Code recomendado)
- Conocimientos básicos de Python y Flask
- Git instalado

## Configuración Inicial
Antes de empezar, asegúrate de tener GitHub Copilot configurado en tu IDE y familiarízate con los comandos básicos de Copilot Chat.

### Ejercicio 0: Migración de Código desde Markdown

**Objetivo**: Migrar el código de un archivo markdown a un proyecto Python estructurado.
Mediante el uso de copilot instructions para añadir contexto extra y aproximar el diseño a una arquitectura limpia.

**Tiempo estimado**: 30 minutos

**Instrucciones**:
1. Crea un nuevo proyecto Python con la siguiente estructura:
   ```
   employee-management/
   ├── src/
   │   ├── models/
   │   ├── repositories/
   │   ├── services/
   │   └── controllers/
   ├── tests/
   ├── requirements.txt
   └── app.py
   ```

2. **Usa GitHub Copilot Chat** para generar el scaffolding del proyecto:
   - Prompt sugerido: "Genera un proyecto Flask para gestión de empleados con arquitectura por capas (modelo, repositorio, servicio, controlador)"
   - Incluye un modelo Employee con campos: id, nombre, apellido, email, departamento
   - Implementa CRUD básico

3. Instala dependencias y verifica que la aplicación ejecuta sin errores:
   ```bash
   pip install -r requirements.txt
   python app.py
   ```

4. **Crea tests unitarios** usando pytest antes de refactorizar:
   - Tests para cada método del servicio
   - Tests para el repositorio
   - Verifica que todos los tests pasan: `pytest tests/`

**Resultado esperado**: Proyecto Flask funcional con tests que pasan.

---

### Ejercicio 1: Refactorización de Métodos

**Objetivo**: Aplicar el principio de responsabilidad única dividiendo métodos largos.

**Tiempo estimado**: 25 minutos

**Concepto clave**: Un método debe hacer una sola cosa y hacerla bien.

**Instrucciones**:
1. **Refactoriza `getAllEmployees`**:
   - Usa Copilot para extraer la lógica de filtrado en `_filter_active_employees()`
   - Extrae la lógica de transformación en `_transform_to_dto()`
   - Prompt sugerido: "Refactoriza este método extrayendo métodos privados para filtrado y transformación"

2. **Refactoriza `saveEmployee`**:
   - Extrae validación en `_validate_employee_data()`
   - Extrae lógica de guardado en `_persist_employee()`

3. **Ejecuta tests**: Verifica que todos siguen pasando tras la refactorización.

**Antes vs Después**: Compara la legibilidad y mantenibilidad del código.

---

### Ejercicio 2: Manejo Robusto de Errores

**Objetivo**: Implementar manejo de excepciones siguiendo buenas prácticas.

**Tiempo estimado**: 20 minutos

**Instrucciones**:
1. **Mejora `getEmployeeById`**:
   - Crea excepción personalizada `EmployeeNotFoundError`
   - Implementa logging de errores
   - Prompt: "Añade manejo de errores robusto con excepciones personalizadas y logging"

2. **Mejora `deleteEmployee`**:
   - Maneja casos de empleado no existente
   - Implementa soft delete si es apropiado
   - Añade validaciones de integridad

3. **Actualiza tests**:
   - Tests para casos de error
   - Verifica que las excepciones se lanzan correctamente
   - Tests para logging

**Patrones a aplicar**: Try-catch específicos, logging estructurado, excepciones descriptivas.

---

### Ejercicio 3: Eliminación de Código Duplicado (DRY)

**Objetivo**: Identificar y extraer lógica repetitiva en métodos reutilizables.

**Tiempo estimado**: 25 minutos

**Instrucciones**:
1. **Extrae búsqueda por email**:
   - Crea `_find_employee_by_email(email: str) -> Employee`
   - Reutiliza en múltiples métodos
   - Prompt: "Identifica código duplicado y extrae métodos helper reutilizables"

2. **Extrae ordenación por apellido**:
   - Crea `_sort_employees_by_lastname(employees: List[Employee]) -> List[Employee]`
   - Implementa ordenación con criterios múltiples

3. **Refactoriza validaciones comunes**:
   - Extrae `_validate_email_format()`
   - Extrae `_validate_required_fields()`

**Tests**: Añade tests específicos para los métodos helper extraídos.

---

### Ejercicio 4: Expansión de Funcionalidades

**Objetivo**: Implementar nuevas features manteniendo la calidad del código.

**Tiempo estimado**: 30 minutos

**Instrucciones**:
1. **Nuevos métodos en el repositorio**:
   - `find_by_name(name: str) -> List[Employee]`
   - `find_by_department(department: str) -> List[Employee]`
   - `get_employees_paginated(page: int, size: int) -> PaginatedResult`

2. **Implementa en el servicio**:
   - `searchEmployeesByName(name: str)`
   - `getEmployeesByDepartment(department: str)`
   - `getEmployeesPaginated(page: int, size: int)`

3. **Añade endpoints REST**:
   - `GET /employees/search?name=xxx`
   - `GET /employees/department/{dept}`
   - `GET /employees?page=1&size=10`

**Tests**: TDD - escribe tests primero, luego implementa.

---

### Ejercicio 5: Documentación y Mejores Prácticas

**Objetivo**: Documentar el código siguiendo estándares profesionales.

**Tiempo estimado**: 20 minutos

**Instrucciones**:
1. **Añade docstrings** siguiendo PEP 257:
   - Usa Copilot para generar docstrings descriptivos
   - Incluye parámetros, retornos y excepciones
   - Prompt: "Genera docstrings completos siguiendo PEP 257 para todos los métodos públicos"

2. **Añade type hints** completos:
   - Usa typing para mayor claridad
   - Implementa validación con pydantic si es necesario

3. **Genera documentación API**:
   - Usa Flask-RESTX o similar para documentación automática
   - Incluye ejemplos de uso

**Bonus**: Genera README.md automáticamente con Copilot incluyendo ejemplos de uso.

---

## Criterios de Evaluación

- [ ] Proyecto ejecuta sin errores
- [ ] Todos los tests pasan
- [ ] Código bien estructurado y legible
- [ ] Manejo adecuado de errores
- [ ] Documentación completa
- [ ] Principios SOLID aplicados

## Recursos Adicionales

- [PEP 8 - Style Guide](https://pep8.org/)
- [Pytest Documentation](https://docs.pytest.org/)
- [Flask Best Practices](https://flask.palletsprojects.com/en/2.3.x/patterns/)

## Próximos Pasos

Después de completar este laboratorio, considera:
- Implementar autenticación y autorización
- Añadir base de datos real (PostgreSQL/MySQL)
- Implementar caching con Redis
- Añadir monitoreo y métricas