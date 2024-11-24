
# Spring Code Kata: Building and Testing a RESTful Web Service

---

### Based on the Spring guide [*Building a RESTful Web Service*](https://spring.io/guides/gs/rest-service), this code kata is designed to help you practice creating and testing a simple REST service. 
---

### **Step 1: Set Up Your Spring Boot Project**
**Objective**: Initialize a Spring Boot project for a REST API.

1. **Task**:
   - Use [Spring Initializr](https://start.spring.io/) to generate a Maven project with the following dependencies:
     - Spring Web
     - Spring Boot DevTools (optional)

2. **Feedback**:
   - Run the application and verify it starts without errors.
   - Validate the default Spring Boot landing page is accessible at `http://localhost:8080`.

---

### **Step 2: Create a REST Endpoint**
**Objective**: Implement a `GET` endpoint that returns a greeting message.

1. **Task**:
   - Create a `GreetingController` class annotated with `@RestController`.
   - Add a `GET` endpoint (`/greeting`) that returns a hardcoded JSON object:
     ```json
     { "id": 1, "content": "Hello, World!" }
     ```

2. **Feedback**:
   - Write a test using `MockMvc` to verify:
     - The endpoint returns `200 OK`.
     - The JSON response structure is as expected:
       ```java
       @Test
       void shouldReturnGreeting() throws Exception {
           mockMvc.perform(get("/greeting"))
                  .andExpect(status().isOk())
                  .andExpect(jsonPath("$.id").value(1))
                  .andExpect(jsonPath("$.content").value("Hello, World!"));
       }
       ```
   - Run the test and fix issues if any.

---

### **Step 3: Add Query Parameters**
**Objective**: Make the greeting message dynamic using query parameters.

1. **Task**:
   - Modify the `/greeting` endpoint to accept a `name` parameter (`?name=Aditya`).
   - Return a personalized message: `"Hello, Aditya!"` if the parameter is provided, otherwise fallback to `"Hello, World!"`.

   Example response for `/greeting?name=Aditya`:
   ```json
   { "id": 1, "content": "Hello, Aditya!" }
   ```

2. **Feedback**:
   - Add a test for this behavior:
     ```java
     @Test
     void shouldReturnPersonalizedGreeting() throws Exception {
         mockMvc.perform(get("/greeting?name=Aditya"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.content").value("Hello, Aditya!"));
     }
     ```

---

### **Step 4: Use a Model for Response**
**Objective**: Refactor the response to use a `Greeting` model class.

1. **Task**:
   - Create a `Greeting` class with `id` and `content` fields.
   - Modify the controller to return an instance of `Greeting`.

2. **Feedback**:
   - Add a unit test to validate the structure and behavior remains unchanged after refactoring.

---

### **Step 5: Add Error Handling**
**Objective**: Handle errors gracefully when the `name` parameter is invalid.

1. **Task**:
   - Add validation logic for `name` (e.g., minimum length or non-empty).
   - Return a `400 Bad Request` with an appropriate error message if validation fails.

2. **Feedback**:
   - Write tests for:
     - Valid `name` producing `200 OK`.
     - Invalid `name` producing `400 Bad Request` with an error response:
       ```java
       @Test
       void shouldReturnBadRequestForInvalidName() throws Exception {
           mockMvc.perform(get("/greeting?name="))
                  .andExpect(status().isBadRequest())
                  .andExpect(content().string("Name parameter must not be empty"));
       }
       ```

---

### **Summary**
This kata ensures you build a foundational REST service incrementally, with immediate feedback provided via automated tests and deployment validation. By the end of these exercises, you'll have mastered the core concepts of creating and validating REST APIs with Spring Boot.
