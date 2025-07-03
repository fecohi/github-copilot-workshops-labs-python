# Employee Management System - Troubleshooting Lab

This project contains a buggy Employee Management System built using Python and Flask. Your task is to use GitHub Copilot to identify and fix various issues in the codebase.

## Pre-requisites

- [Visual Studio Code](https://code.visualstudio.com/) or any other editor that supports GitHub Copilot.
- [GitHub Copilot](https://copilot.github.com/) extensions installed.
- Python 3.8 or higher
- Pytest
- Flask

## Project Structure

```
python/
├── __init__.py
├── app.py
├── controllers/
│   └── employee_controller.py
├── models/
│   └── employee.py
├── routes/
│   └── employee_routes.py
├── test_employee.py
├── requirements.txt
└── README.md
```

## Getting Started

1. **Navigate to the Python project:**
   ```
   cd python
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

4. **Attempt to run the application:**
   ```
   python app.py
   ```

## Troubleshooting Challenges

This application contains various bugs and issues that need to be identified and fixed using GitHub Copilot:

### 1. Debug and solve runtime errors

In this challenge, you will need to debug and solve runtime errors in the project. Use GitHub Copilot to help you identify the root cause of the errors and provide the necessary fixes.

**Expected result:** An operative Flask application with no runtime errors.

### 2. Fix functional errors

Once the application runs, test all the endpoints and fix any functional errors that you find.

**Expected result:** A fully functional API with all endpoints working correctly.

### 3. Testing and Validation

Review the test cases and add new ones if necessary. Debug any failing tests and ensure that all test cases pass.

**Expected result:** A complete test suite with all tests passing.

### 4. Performance and Logic Issues

Identify and fix any performance bottlenecks or logic errors in the application using GitHub Copilot to suggest optimizations.

## Expected API Endpoints (once fixed)

- `GET /employees` - List all employees
- `POST /employees` - Add a new employee
- `DELETE /employees/<id>` - Delete an employee by ID
- `PUT /employees/<id>` - Update an employee by ID


## Testing Your Fixes with curl

### 1. Create a New Employee
```sh
curl -X POST http://localhost:5000/employees -H "Content-Type: application/json" -d '{
  "name": "John Doe",
  "position": "Software Engineer",
  "department": "Engineering"
}'
```

### 2. Get All Employees
```sh
curl -X GET http://localhost:5000/employees
```

### 3. Get Employee by ID
```sh
curl -X GET http://localhost:5000/employees/{id}
```
Replace `{id}` with the actual employee ID.

### 4. Update an Existing Employee
```sh
curl -X PUT http://localhost:5000/employees/{id} -H "Content-Type: application/json" -d '{
  "name": "Jane Doe",
  "position": "Senior Software Engineer", 
  "department": "Engineering"
}'
```
Replace `{id}` with the actual employee ID.

### 5. Delete an Employee
```sh
curl -X DELETE http://localhost:5000/employees/{id}
```
Replace `{id}` with the actual employee ID.

## License

This project is licensed under the MIT License.