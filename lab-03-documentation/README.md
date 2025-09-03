# GitHub copilot documentation lab

This repository contains a collection of challenges to improve your skills with GitHub Copilot regarding documentation and related topics.

## Pre-requisites

- [Visual Studio Code](https://code.visualstudio.com/) or any other editor that supports GitHub Copilot.
- [GitHub Copilot](https://copilot.github.com/) extensions installed.
- Python 3.10 or higher

## Employee Management System

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
Document the codebase in detail:

* Add descriptive module, class and method docstrings to `employee.py`, `employee_controller.py`, `employee_routes.py`, and `app.py`.
* Ensure each docstring explains purpose, parameters, return values and raises (if any).

Hint: remember to use Control + I to bring the inline editor.

### 3. API Documentation Export
Update and publish the documentation:

1. After the Swagger docstrings are in place, generate the OpenAPI spec (`swagger.json`).
2. Produce a markdown file in a new `docs/` folder:
   * `technical.md` – Full OpenAPI YAML/JSON plus class/method docstrings for deeper technical understanding.

# Bonus

## GitHub Copilot Best Practices

To make the most of GitHub Copilot in your development workflow, follow these best practices. For each, you will find a task to practice, as well as examples of both bad and good practices.


### Be Specific in Prompts

**Advice:**  
The more context you provide in comments or code, the better Copilot’s suggestions will be.

**Task:**  
Write a function that calculates the factorial of a number. First, use a vague comment or no comment. Then, write a clear comment describing exactly what you want, including parameter types and return value.

**Bad Practice**
```python
# calculate
def factorial(n):
    pass  # Copilot may generate irrelevant code
```

**Good Practice**
```python
def factorial(n: int) -> int:
    """Calculate the factorial of a non-negative integer n using recursion.
    Return 1 if n is 0. Raise ValueError for negative input.
    """
    pass
```

---

### Review and Edit Suggestions

**Advice:**  
Always review Copilot outputs. Do not accept code without reading it for logic, security, and style.

**Task:**  
Generate a function with Copilot that parses a number from a string. Review the suggestion and edit it if it does not handle errors or edge cases correctly.

**Bad Practice**
```python
def parse_int(s: str) -> int:
    return int(s)  # No error handling
```

**Good Practice**
```python
from typing import Optional

def parse_int(s: str) -> Optional[int]:
    """Parse an integer from string s. Return None on failure."""
    try:
        # Accept underscores, strip whitespace
        return int(s.strip().replace("_", ""))
    except (ValueError, AttributeError):
        return None
```

---

### Use Incremental Prompts

**Advice:**  
Break complex tasks into smaller parts and prompt Copilot for each. Do not expect a multi-step solution from one prompt.

**Task:**  
First, ask Copilot to implement user authentication logic in a single comment. Then, split the implementation into:  
a function for verifying a username, a function for password verification, and a function for combining the results.

**Bad Practice**
```python
# authenticate user
def authenticate(username: str, password: str):
    # Copilot might mix concerns and produce hard-to-test code
    ...
```

**Good Practice**
```python
def username_exists(username: str, users: Mapping[str, bytes]) -> bool:
    """Check whether the username exists in the user store."""
    pass

def verify_password(password: str, salt: bytes, expected_hash: bytes) -> bool:
    """Verify password using PBKDF2-HMAC. Returns True on match."""
    pass

def authenticate(username: str, password: str,
                 users: Mapping[str, tuple[bytes, bytes]]) -> bool:
    """Return True only if username exists and password verifies."""
    pass
```

---

### Follow Coding Standards

**Advice:**  
Make sure Copilot’s code matches your project’s style and naming conventions.

**Task:**  
Accept a Copilot-generated function and adjust formatting and naming to match the style of the surrounding code.

**Bad Practice**
```python
def Get_User_ID(NAME):
    # Inconsistent naming and formatting
    ...
```

**Good Practice**
```python
def get_user_id(name: str) -> int:
    """Return the user id for a given name, or raise if not found."""
    ...
```

---

### Leverage Copilot for Learning, but Verify Outputs

**Advice:**  
Use Copilot to explore alternatives and new patterns, always check the logic.

**Task:**  
Ask Copilot for two different ways to reverse a string. Test both methods and explain which you prefer and why.

**Bad Practice:**  
Accept code without checking for empty or non-string inputs.

**Good Practice**
Test the suggestions with different inputs, and add null/empty checks as needed.

---

### Avoid Sensitive Data

**Advice:**  
Never paste API keys, passwords, or confidential information in prompts.

**Task:**  
Write a function that connects to a database. Use environment variables for credentials instead of embedding them in the code or prompts.

**Bad Practice**
```python
# Connect to db
USERNAME = "admin"
PASSWORD = "SuperSecret123"  # Hard-coded credentials
```

**Good Practice**
```python
# Connect to db using credentials from environment variables  
```

---

### Combine Copilot with Other Tools

**Advice:**  
Use static analyzers, linters, and tests to ensure Copilot’s code is safe and well-structured.

**Task:**  
Generate a Copilot function, then run your linter or static analyzer and correct any warnings or errors reported.

---

### Iterate with Feedback

**Advice:**  
If Copilot’s suggestion is not right, revise your comment or try again for better results.

**Task:**  
Write a prompt for a function, review the suggestion, and, if incorrect, refine your prompt until Copilot provides a suitable solution.

## License

This project is licensed under the MIT License.