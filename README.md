# node_express_mongo_docker

This is a simple **Node.js Express API** that connects to a **MongoDB** database to retrieve user data. The API provides a **GET** endpoint to fetch a user by ID, but only returns users **older than 21**.

## Features
- Uses **MongoDB** as the database
- **Express.js** for the web framework
- **Docker support** for containerized deployment
- **K8S manifest** for running in a Kubernetes cluster

## API Endpoints
### GET `/users/:id`
- **Description**: Retrieves user data by ID (only if age > 21)
- **Request Parameters**:
  - `id`: The MongoDB ObjectId of the user
- **Response**:
  - `200 OK`: Returns user data
  - `404 Not Found`: If user is not found or does not meet age criteria
  - `400 Bad Request`: If invalid ObjectId is provided

### Example Response
```json
{
  "_id": "60c72b2f9fd3c2a5d8f6b8d2",
  "name": "John Doe",
  "email": "johndoe@email.com",
  "age": 30
}
```

## How to Run

### Run Locally (Without Docker)
#### **Prerequisites**
- **Node.js** (>= 18.x)
- **MongoDB** (running locally or in the cloud)

#### **Steps**
1. **Clone the repo**:
   ```sh
   git clone https://github.com/c0delifter/node_express_mongo_docker.git
   cd express-api
   ```
2. **Install dependencies**:
   ```sh
   npm install
   ```
3. **Set environment variables** (create a `.env` file):
   ```
   MONGO_URI=mongodb://localhost:27017/mydatabase
   PORT=3000
   ```
4. **Start the API**:
   ```sh
   npm start
   ```
5. **Test the API**:
   ```sh
   curl http://localhost:3000/users/60c72b2f9fd3c2a5d8f6b8d2
   ```

---
### Run Using Docker
#### **Prerequisites**
- **Docker** installed

#### **Steps**
1. **Build the Docker image**:
   ```sh
   docker build -t my-node-api .
   ```
2. **Run the container**:
   ```sh
   docker run -p 3000:3000 --env MONGO_URI=mongodb://host.docker.internal:27017/mydatabase my-node-api
   ```
3. **Test the API** (same as above)

---
### Run Using Kubernetes
#### **Prerequisites**
- **Minikube** or a Kubernetes cluster

#### **Steps**
1. **Apply the deployment & service**:
   ```sh
   kubectl apply -f k8s-deployment.yaml
   ```
2. **Get the external LoadBalancer IP**:
   ```sh
   kubectl get svc
   ```
3. **Test the API**:
   ```sh
   curl http://<EXTERNAL_IP>/users/60c72b2f9fd3c2a5d8f6b8d2
   ```

---
## Testing Strategies
### 1. **Unit Testing (Jest + Supertest)**
- Install dependencies:
  ```sh
  npm install --save-dev jest supertest
  ```
- Example test file (`users.test.js`):
  ```js
  const request = require('supertest');
  const app = require('../server');

  test('GET /users/:id - valid user', async () => {
    const response = await request(app).get('/users/60c72b2f9fd3c2a5d8f6b8d2');
    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('name');
  });
  ```
- Run tests:
  ```sh
  npm test
  ```

### 2. **Integration Testing (Postman, Insomnia)**
- Use **Postman** or **Insomnia** to manually test the API
- Send GET requests to `http://localhost:3000/users/:id`

### 3. **Load Testing (k6)**
- Install k6:
  ```sh
  brew install k6
  ```
- Run a simple load test:
  ```sh
  k6 run load-test.js
  ```

---
## License
This project is licensed under the MIT License.

