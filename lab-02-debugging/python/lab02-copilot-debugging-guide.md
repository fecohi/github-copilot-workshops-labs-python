# 🧪 Guía de Resolución: Laboratorio 02 - Depuración con GitHub Copilot

**Repositorio:** https://github.com/Geronimo-Basso/github-copilot-workshops-labs-python/tree/main/lab-02-debugging/python  
**Objetivo:** Aprender a depurar aplicaciones Python utilizando GitHub Copilot Chat como asistente inteligente.

---

## 🔧 Preparación del Entorno

```sh
git clone https://github.com/Geronimo-Basso/github-copilot-workshops-labs-python.git  
cd github-copilot-workshops-labs-python/lab-02-debugging/python

python -m venv venv  
. venv/Scripts/activate

pip install -r requirements.txt  
python .\app.py
```

---

## 🧩 Ejercicio 1: Debug and solve runtime errors

1. Abrir **GitHub Copilot Chat**.
2. Activar el **Modo ASK** con el modelo deseado.
3. Arrastrar la carpeta `python` al entorno de Copilot o usar `@workspace` al inicio del prompt.
4. Usa los siguientes **PROMPTS** sugeridos:

   - **PROMPT:** `@terminal /explain`  
     *Solicita una explicación del último comando ejecutado.*

   - **PROMPT:** `@workspace help me to identify the route cause of this error and provide me any necessary fix #terminalLastCommand`  
     *Identifica la causa raíz del error y aplica correcciones sugeridas.*

5. Aplicar la corrección sugerida en `app.py`:  
   Cambiar `emp` por `employee_routes`.

6. Ejecutar nuevamente:

```sh
   python app.py
```

7. Si el navegador muestra **"Not Found"**, usa este **PROMPT**:

   - **PROMPT:**  
     `I was able to execute without errors python app.py but now the browser says: Not Found. The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.`

8. Añadir una ruta raíz para pruebas (opcional):

```python
   @app.route('/')  
   def home():  
       return {  
           "message": "Welcome to the Employee API",  
           "endpoints": {  
               "GET /employees": "List all employees",  
               "POST /employees": "Add a new employee",  
               "GET /employees/<id>": "Get employee by ID",  
               "GET /employees/<email>": "Get employee by email",  
               "DELETE /employees/<id>": "Delete employee",  
               "PUT /employees/<id>": "Update employee"  
           }  
       }
```

9. Mejorar presentación HTML:

   - **PROMPT:**  
     `please update the welcome/home route to be more html user-friendly with better formatting and a cleaner presentation`

---

## 🧩 Ejercicio 2: Fix functional errors

1. Acceder al endpoint `/employee` y verificar errores.
2. Arrastrar nuevamente la carpeta `python` al entorno de Copilot.
3. **PROMPT:**  
   `I am getting an error accessing to the /employee endpoint: <COPY STACKTRACE>`
4. Aplicar corrección sugerida por Copilot.
5. Ejecutar nuevamente:

   localhost:5000/employees

6. Validar que no haya errores en terminal.

### 🔁 Pruebas con `curl`:

```sh
curl -X POST http://localhost:5000/employees -H "Content-Type: application/json" -d '{  
  "name": "John Doe",  
  "position": "Software Engineer",  
  "department": "Engineering"  
}'

curl -X GET http://localhost:5000/employees  
curl -X GET http://localhost:5000/employees/1

curl -X PUT http://localhost:5000/employees/1 -H "Content-Type: application/json" -d '{  
  "name": "Jane Doe",  
  "position": "Senior Software Engineer",  
  "department": "Engineering"  
}'

curl -X GET http://localhost:5000/employees/1  
curl -X DELETE http://localhost:5000/employees/1
```

7. Si hay errores:

   - **PROMPT:**  
     `something is wrong trying to post a new employee, please check the error in the terminal #terminalLastCommand`

---

## 🧩 Ejercicio 3: Testing and Validation

1. Instalar `pytest`:

```sh
   pip install pytest
```

2. Abrir `test_employee.py`.
3. Ejecutar los tests:

```sh
   python -m pytest test_employee.py -v
```

4. Arrastrar la carpeta `python` y abrir nueva sesión en Copilot.

### PROMPTS sugeridos:

- **PROMPT:** `@terminal /explain`  
- **PROMPT:** `help me to fix the error to be able to execute pytest #terminalLastCommand`  
- **PROMPT:** `please help me understand why my tests are not passing #terminalLastCommand`  
- **PROMPT:** `Debug any failing tests and fix any test to ensure that all test cases pass #terminalLastCommand`

---

## 🧩 Ejercicio 4: Performance and Logic Issues

- **PROMPT:**  
  `Identify and fix any performance bottlenecks or logic errors in the application`
