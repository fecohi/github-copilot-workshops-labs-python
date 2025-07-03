# Construcción de la API de Pokémon desde cero

Este documento describe los pasos realizados para construir la API de Pokémon desde cero, junto con los comandos necesarios para ejecutarla y configurarla correctamente.

## Pre-requisitos

- [Visual Studio Code](https://code.visualstudio.com/) u otro editor que soporte GH Copilot.
- [GitHub Copilot](https://copilot.github.com/) extensiones instaladas.
- [Fast API](https://pypi.org/project/fastapi/) o en su defecto [Flask](https://pypi.org/project/Flask/).
- [PyTest](https://pypi.org/project/pytest/).
- [Uvicorn](https://pypi.org/project/uvicorn/).
- [Python](https://www.python.org/downloads/) Version >3.8

## Lab Overview
En este laboratorio, crearás una aplicación sencilla pero funcional con FastAPI que implemente operaciones CRUD (crear, leer, actualizar y eliminar) y, además, la someterás a pruebas unitarias usando pytest. Para ello, usarás GitHub Copilot como asistente para generar código y resolver los errores que se vayan presentando (tanto de sintaxis, importaciones y cualquier otro inconveniente). Puedes ser todo lo creativo que quieras.

## Pasos para construir la aplicación
Es recomendado usar un entorno virtual para que no haya error en las dependencias con nuestro entorno loccal.

1. **Crear el entorno de trabajo**:
   - Crear un directorio para la aplicación.
   - Inicializar un entorno virtual de Python:
     ```bash
     python -m venv venv
     ```
   - Activar el entorno virtual:
     - En Windows:
       ```bash
       venv\Scripts\activate
       ```
     - En macOS/Linux:
       ```bash
       source venv/bin/activate
       ```

   - Alternativamente, puedes usar Conda para crear y gestionar el entorno:
     - Crear un nuevo entorno con Conda:
       ```bash
       conda create --name pokemon-api python=3.8
       ```
     - Activar el entorno:
       ```bash
       conda activate pokemon-api
       ```
**Estructura objetivo**:
Lo primero que se quiere conseguir, es generar la estructura del proyecto con el que trabajaremos. Una arquitectura de ejemplo puede ser esta:
   - Crear la estructura de directorios y archivos:
     ```
     pokemon-api/
     ├── app/
     │   ├── __init__.py
     │   ├── app.py
     │   ├── models.py
     │   ├── routes.py
     │   └── test_routing.py
     ├── requirements.txt
     └── README.md
     ```
     -  TIP: Se puede usar custom_instructions.md para crear cierto contexto para Copilot a la hora de pedirle acciones. Prueba a escribir qué estructura quieres que tengan tus tests o cómo quieres que te comente las funcionalidades. Añade ejemplos si es necesario.

**Instalar dependencias**:
   - En el archivo `requirements.txt` , pidele que añada las dependencias necesarias, en este caso:
     ```
     fastapi
     uvicorn
     pytest
     ```



**Desarrollar la aplicación**:
   - Implementar la lógica principal en `app.py`. Intenta pedirle a Copilot que al ejecutarse la app funcione correctamente con uvicorn.
   
   - Definir la clase Pokemon en `models.py`. Pídele a Copilot que te genere una clase llamada Pokemon y que tenga atributos como id, nombre del pokemon y su tipo.

   - Configurar las rutas en `routes.py`. Pídele a Copilot que te genere las rutas de la API. Unos ejemplos de rutas pueden ser GET todos los pokemon, GET pokemon por id, POST pokemon, DELETE pokemon por id o GET buscar pokemon por nombre...


**Ejecutar la aplicación**:
   - Instalar las dependencias. Situarse dentro de la carpeta donde este el archivo:
     ```bash
     pip install -r requirements.txt
     ```
   - Iniciar el servidor FastAPI con Uvicorn. Situarse en `/pokemon-api`:
     ```bash
     uvicorn app.app:app --reload
     ```
   - La aplicación estará disponible en `http://127.0.0.1:8000/` por defecto.

**Probar las rutas**:
Con Copilot, genera comandos curl para probar las distintas rutas que hayas creado.

**Generacion de test**:
Crea un archivo nuevo para el testeo llamado test_routes.py con Copilot. Pídele que te genere los test necesarios para probar tus funcionalidades.

   - Ejecutar las pruebas unitarias con `pytest`:
     ```bash
     pytest app/test_routing.py
     ```

        - TIP: si no te pasan todos los tests, prueba a pasarle los logs de error a Copilot para que haga troubleshooting del posible error. Puedes usar custom_instructions.md para que darle cierto formato a tus tests.

**Cobertura de codigo**:
Píde a Copilot que quieres saber la cobertura de código de tus tests. 

- TIP: Puedes usar `pytest-cov` para esto. Instala la dependencia en tu entorno virtual:
```bash
pip install pytest-cov
```

**Crear documentacion**:
Si no se ha generado un README.md, genéralo con Copilot. Pídele que cree documentación de tu app.

**Avanzado**:

***Implementar servicio externo para datos de Pokémon***:
  Pídele a Copilot que implemente un servicio externo que devuelva datos de Pokémon. Puedes usar una API pública como `https://pokeapi.co/api/v2`.
  
  Recuerda añadir un carpeta `services` y un archivo `pokemon_service.py` donde se implementará la lógica para consumir esta API.

***Amplia el modelo de Pokémon***:
  Pídele a Copilot que amplíe el modelo de Pokémon para incluir más atributos, como habilidades, estadísticas, etc. 
  
  Actualiza la clase `Pokemon` en `models.py` y ayudate de Copilot para ajustar las rutas y tests según sea necesario.

***Separar la logica de datos en un repositorio***:
  Crea un repositorio de datos para manejar la persistencia de los Pokémon. Pídele a Copilot que extraiga a un repositorio en `app/repository/pokemon_repository.py` la lógica de datos que tenemos en nuestra app para que maneje las operaciones definidas anteriormente.
- NOTA: No olvides actualizar las rutas y tests para reflejar estos cambios.

***Añade tests con mocks***:
  Pídele a Copilot que implemente tests unitarios simulando por ejemplo la respuesta de la API externa de Pokémon. 

***Tests parametrizados parar mejorar cobertura***:
  Pídele a Copilot que implemente tests parametrizados para cubrir diferentes casos de uso en tus rutas. Puedes usar `pytest.mark.parametrize` para esto.
  
  - Recuerda revisar tu cobertura de código después de añadir estos tests.

**Actualiza la documentacion**:
Pídele a Copilot que actualice la documentacion de tu app con los últimos cambios que has realizado. Recuerda que puedes usar custom_instructions.md para darle cierto formato a la documentación.

## Recuerda
Puedes ser todo lo creativo que quieras y elevar este laboratorio a la experiencia que tengas desarrollando. Prueba a crear mas clases, funcionalidades extra, añade autenticacion, o tests si así lo deseas.
