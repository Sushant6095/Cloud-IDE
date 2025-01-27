# Cloud IDE

A cloud-native Integrated Development Environment (IDE) built using cutting-edge technologies like Kubernetes (K8s), Docker, Node.js ,Next.js and AWS. Designed with scalability, isolation, and seamless developer experience in mind, this Cloud IDE allows users to edit code, access terminals, and execute code in isolated environments. The system leverages WebSockets, xterm.js, and PTY for terminal emulation, with a robust architecture comprising several microservices.

---

## Features

- **Scalable Architecture**: Built using Kubernetes and Docker to ensure high scalability and isolation.
- **Cloud-Native Approach**: Environment isolation for secure and efficient resource utilization.
- **Terminal Access**: Real-time terminal interaction using PTY and xterm.js.
- **File Editing and Execution**: Edit files and execute code via WebSockets for real-time collaboration.
- **Automatic Cleanup**: Pods and resources are automatically cleaned up after inactivity (~5 minutes with 0 WebSocket connections).
- **Auto-Destruction of Pods**: If a user is inactive for ~5 minutes, the associated pod is automatically destroyed to free up resources.
  

---

## Tech Stack

### Core Technologies
- **Kubernetes (K8s)**: Orchestrates the pods for isolated environments.
- **Docker**: Provides containerized environments for user sessions.
- **AWS S3**: Stores base images and other assets.
- **Node.js & TypeScript**: Backend services and business logic.
- **Next.js**: Frontend for user interaction.
- **xterm.js**: Terminal emulation.
- **WebSocket**: Real-time communication for terminal and file execution.

---

## Architecture

The Cloud IDE is divided into four main services:

### 1. **Frontend**
- Built with **Next.js**.
- Provides a responsive and user-friendly interface for:
  - Code editing.
  - Terminal access.
  - Real-time feedback on execution.

### 2. **Init Service**
- Initializes the environment by copying the base image to AWS S3 and setting up the necessary configurations.
-  Prepares the environment for user sessions..
- Manages the AWS configuration via `aws.ts`.

### 3. **Runner Service**
- WebSocket-based service for terminal code execution.
- Features:
  - Starts an independent runner for each user session.
  - Displays a loading screen while the environment initializes.

### 4. **Orchestrator**
- Manages pods and environment orchestration:
  - Tells Kubernetes to start a pod for each user.
  - Ensures environment isolation and efficient resource allocation.
  - Destroys pods when no active WebSocket connections exist for ~5 minutes.

### 5. **Execution Service**
- Handles code execution and terminal access.
- Enables users to edit files and execute code over the WebSocket layer.
- Cleans up resources by destroying idle pods.

---

## Getting Started

### Prerequisites

- **Node.js** (v16+)
- **Docker**
- **Kubernetes**
- **AWS CLI**
- **S3 Bucket** (for storing base images)

### How It Works
-**User Access**: A user accesses the Cloud IDE through the frontend (Next.js application).

-**Environment Initialization**: The Init Service prepares the environment by copying the base image to AWS S3.

-**Runner Initialization**: The Runner Service starts an independent runner for the user, establishing a WebSocket connection for terminal access and code execution.

-**Pod Creation**: The Orchestrator starts a Kubernetes pod for the user session.

-**Code Execution**: The user writes and edits code in the frontend. The code is sent to the Execution Service via WebSockets, where it is executed in the isolated environment.

-**Terminal Access**: The user can access a terminal in the browser using xterm.js, which communicates with the backend via WebSockets.

-**Auto-Destruction**: If the user is inactive for ~5 minutes, the Orchestrator destroys the associated pod to free up resources.











### Clone the Repository

```bash
git clone https://github.com/your-username/cloud-ide.git
cd cloud-ide
```

### Install Dependencies

Navigate to each folder (frontend, init-service, runner-service, orchestrator, execution-service) and install dependencies:

```bash
cd frontend
npm install

cd ../init-service
npm install

cd ../runner-service
npm install

cd ../orchestrator
npm install

cd ../execution-service
npm install
```

### Environment Variables

Create a `.env` file in each service folder with the required configurations. Example for the init service:

```env
AWS_REGION=your-region
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
S3_BUCKET=your-bucket-name
```

### Run Services Locally

- **Frontend**:
  ```bash
  cd frontend
  npm run dev
  ```

- **Init Service**:
  ```bash
  cd init-service
  npm start
  ```

- **Runner Service**:
  ```bash
  cd runner-service
  npm start
  ```

- **Orchestrator**:
  ```bash
  cd orchestrator
  npm start
  ```

- **Execution Service**:
  ```bash
  cd execution-service
  npm start
  ```

---

## Deployment

### Docker

Build and push Docker images for each service:

```bash
cd service-folder
docker build -t your-docker-repo/service-name .
docker push your-docker-repo/service-name
```

### Kubernetes

Deploy services to Kubernetes:

1. Create a Kubernetes namespace:
   ```bash
   kubectl create namespace cloud-ide
   ```

2. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f k8s-manifests/
   ```

3. Verify the pods:
   ```bash
   kubectl get pods -n cloud-ide
   ```

---

## API Endpoints

### Runner Service
| Method | Endpoint          | Description             |
|--------|-------------------|-------------------------|
| GET    | `/start`          | Start a new runner.     |
| POST   | `/execute`        | Execute user commands.  |
| DELETE | `/destroy/:id`    | Destroy a runner pod.   |

### Orchestrator
| Method | Endpoint          | Description                     |
|--------|-------------------|---------------------------------|
| POST   | `/orchestrate`    | Start a pod for a user session. |
| DELETE | `/cleanup/:id`    | Destroy inactive pods.          |

---

## Contributing

We welcome contributions to improve this Cloud IDE. Follow these steps:

1. Fork the repository.
2. Create a feature branch:
   ```bash
   git checkout -b feature-name
   ```
3. Commit your changes:
   ```bash
   git commit -m 'Add feature-name'
   ```
4. Push the branch:
   ```bash
   git push origin feature-name
   ```
5. Open a pull request.

---

## License

This project is licensed under the [MIT License](LICENSE).

---


