# Employee Management System

This project is an Employee Management System built using Python and Flask. It provides a simple API to manage employee records, allowing for operations such as listing, adding, deleting, and updating employee information.

## Project Structure

```
lab-03-documentation
├── __init__.py
├── app.py
├── employee_controller.py
├── employee.py
├── employee_routes.py
├── requirements.txt
└── README.md
```

## Setup Instructions

1. **Clone the repository:**
   ```
   git clone <repository-url>
   cd employee-management
   ```

2. **Create a virtual environment:**
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install the required packages:**
   ```
   pip install -r requirements.txt
   ```

4. **Run the application:**
   ```
   python app/app.py
   ```

## API Endpoints

- `GET /employees` - List all employees
- `POST /employees` - Add a new employee
- `DELETE /employees/<id>` - Delete an employee by ID
- `PUT /employees/<id>` - Update an employee by ID

## Usage Examples

### List Employees
```
GET /employees
```

### Add Employee
```
POST /employees
{
    "name": "John Doe",
    "position": "Software Engineer",
    "department": "Engineering"
}
```

### Delete Employee
```
DELETE /employees/1
```

### Update Employee
```
PUT /employees/1
{
    "name": "John Smith",
    "position": "Senior Software Engineer",
    "department": "Engineering"
}
```

## Example curl Commands

### List all employees
```sh
curl -X GET http://localhost:5000/employees
```

### Add a new employee (with id)
```sh
curl -X POST http://localhost:5000/employees \
   -H "Content-Type: application/json" \
   -d '{"id": 1, "name": "John Doe", "position": "Software Engineer", "department": "Engineering"}'
```

### Delete an employee by ID
```sh
curl -X DELETE http://localhost:5000/employees/1
```

### Update an employee by ID (with id in body)
```sh
curl -X PUT http://localhost:5000/employees/1 \
   -H "Content-Type: application/json" \
   -d '{"id": 1, "name": "John Smith", "position": "Senior Software Engineer", "department": "Engineering"}'
```

## Workshop Tasks

### 1. Swagger
Add Swagger (Flasgger) to the project so the entire API is self-documented and can be tested through the built-in Swagger UI.

1. Initialize Swagger in `app.py`.
2. Write a Flasgger YAML docstring for every endpoint in `employee_routes.py`.
3. Verify that `/apidocs/` renders the Swagger UI and all routes are listed.

### 2. Code Documentation
Document the codebase thoroughly:

* Add descriptive module, class and method docstrings to `employee.py`, `employee_controller.py`, `employee_routes.py`, and `app.py`.
* Ensure each docstring explains purpose, parameters, return values and raises (if any).

Hint: remember to use Control + I to bring the inline editor.

### 3. API Documentation Export
Update and publish the documentation:

1. After the Swagger docstrings are in place, generate the OpenAPI spec (`swagger.json`).
2. Produce a markdown file in a new `docs/` folder:
   * `technical.md` – Full OpenAPI YAML/JSON plus class/method docstrings for deeper technical understanding.

## License

This project is licensed under the MIT License.