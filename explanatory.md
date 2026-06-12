# Beginner Explanatory Guide: DATA-202: Build User Management REST API

> **Task Type**: Service Task  
> **Domain/Focus**: API endpoints, Python fundamentals

---

## 1. The Goal (In-Depth Beginner Explanation)

### The Core Problem
The task at hand is to build a User Management REST API, which is a crucial component for any application that requires user interaction, such as registration, login, and profile management. Currently, the application lacks the ability to manage users effectively, which means that users cannot be created, retrieved, updated, or deleted. This limitation is problematic because it prevents the application from functioning as intended, leading to a poor user experience. 

Without a proper user management system, users cannot register or log in, which is essential for personalized experiences in applications. This task aims to implement five key API endpoints that will allow the application to handle user data efficiently. By completing this task, we will enable the application to manage users, ensuring that it can scale and provide necessary functionalities to its users.

### Jargon Buster (Key Terms Explained)
* **REST API**: REST stands for Representational State Transfer. It is an architectural style for designing networked applications. A REST API allows different software systems to communicate over the internet using standard HTTP methods (like GET, POST, PUT, DELETE). For example, when you use a web application to fetch user data, it often does so by making a GET request to a REST API endpoint.

* **CRUD**: CRUD stands for Create, Read, Update, and Delete. These are the four basic operations that can be performed on data. In the context of our User Management API, creating a user corresponds to the "Create" operation, retrieving user information corresponds to "Read," modifying user details corresponds to "Update," and removing a user corresponds to "Delete."

* **HTTP Status Codes**: These are standardized codes returned by a server to indicate the result of a client's request. For instance, a status code of 200 means the request was successful, while a 404 indicates that the requested resource was not found. Understanding these codes is crucial for debugging and ensuring that the API behaves as expected.

* **Input Validation**: This is the process of ensuring that the data provided by users meets certain criteria before it is processed. For example, when creating a user, we need to validate that the name is not empty and that the email is in a valid format. This helps prevent errors and ensures data integrity.

### Expected Outcome
After implementing the solution, the User Management API should function correctly, allowing users to be created, retrieved, updated, and deleted. 

**Before vs. After**:
- **Before**: The application cannot manage users; attempts to create, retrieve, update, or delete users result in errors or no functionality.
- **After**: The application successfully handles user management through five functional API endpoints, returning appropriate HTTP status codes and messages based on the actions performed.

---

## 2. Related Coding Concepts & Syntax (50% Theory, 50% Practice)

### Concept 1: Input Validation
#### 📘 Theoretical Overview (50%)
* **Why it exists**: Input validation is essential to ensure that the data received from users is correct and safe to process. Without validation, applications can encounter errors, behave unpredictably, or even become vulnerable to security threats such as SQL injection or cross-site scripting (XSS). For instance, if a user submits an email address without the "@" symbol, the application should reject this input to prevent further issues.

* **Key Mechanisms**: Input validation typically involves checking the format, length, and type of the data. For example, we might check that a user's name is a string and does not exceed a certain length, and that an email matches a specific pattern (like the one defined in `constants.py`).

#### 💻 Syntax & Practical Examples (50%)
* **Language Syntax**:
  ```python
  import re

  def validate_email(email: str) -> bool:
      pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
      return re.match(pattern, email) is not None
  ```
  In this example, we use the `re` module to check if the email matches the defined pattern. The `match` function returns a match object if the pattern is found; otherwise, it returns `None`.

* **Real-World Application**:
  ```python
  def create_user(data: dict) -> dict:
      if 'name' not in data or not data['name']:
          return {'error': 'Name is required', 'status': 400}
      if 'email' not in data or not validate_email(data['email']):
          return {'error': 'Invalid email format', 'status': 400}
      # Proceed to create user
  ```
  Here, we validate that the `name` field is present and not empty, and that the `email` field is valid before proceeding to create a user.

---

## 3. Step-by-Step Logic & Walkthrough

1. **Step 1: Locate and Analyze the Target File**
   * Navigate to the `userAPI.py` file within the `s-w10-task-04` folder. This file contains the class `UserAPI`, which has stubs for the CRUD operations we need to implement.
   * Focus on the methods: `list_users`, `get_user`, `create_user`, `update_user`, and `delete_user`. These methods are where we will add our logic.

2. **Step 2: Input Verification & Validation**
   * In the `create_user` method, check if the `name` field is present and not empty. If it is missing or empty, return a 400 status code with an appropriate error message.
   * Validate the `email` field using the `_validate_email` method to ensure it matches the required format. If it fails validation, return a 400 status code.

3. **Step 3: Core Implementation / Modification**
   * Implement the logic to create a new user:
     - Assign an auto-incremented ID to the new user.
     - Store the user data in the `self.users` dictionary.
     - Return the user data along with a 201 status code.
   * Implement similar logic for the other methods (`get_user`, `update_user`, `delete_user`) to handle retrieving, updating, and deleting users, ensuring to return appropriate status codes (200, 204, 404).

4. **Step 4: Output Verification & Testing**
   * After implementing the methods, run the test suite in `test_api.py` using pytest to ensure all tests pass. This will verify that the API behaves as expected and meets the acceptance criteria outlined in the task description.

---

## 4. Detailed Walkthrough of Test Cases

### Test Case 1: Standard / Success Case
* **Description**: This test checks the successful creation of a user.
* **Inputs**:
  ```json
  {
      "name": "Alice",
      "email": "alice@test.com"
  }
  ```
* **Step-by-Step Execution Trace**:
  1. The `create_user` function receives the input values.
  2. It checks if the `name` is present and not empty, which evaluates to true.
  3. It validates the email format using the `_validate_email` method, which also evaluates to true.
  4. The function assigns an ID of 1 to the new user, stores the user data, and returns the user object along with a status of 201.
* **Expected Output**: 
  ```json
  {
      "user": {
          "id": 1,
          "name": "Alice",
          "email": "alice@test.com"
      },
      "status": 201
  }
  ```

### Test Case 2: Edge Case / Validation Fail
* **Description**: This test checks the scenario where the user tries to create a user without providing a name.
* **Inputs**:
  ```json
  {
      "email": "test@test.com"
  }
  ```
* **Step-by-Step Execution Trace**:
  1. The `create_user` function receives the input values.
  2. It checks if the `name` is present, which evaluates to false.
  3. The function immediately returns an error message indicating that the name is required, along with a status of 400.
* **Expected Output**: 
  ```json
  {
      "error": "Name is required",
      "status": 400
  }
  ```