## Overview
#### This project is a full-stack CRUD application built with the MEAN stack (MongoDB, Express, Angular 15, Node.js). It manages a collection of tutorials with fields: id, title, description, and published. Users can create, retrieve, update, delete, and search tutorials by title.​

#### The application is fully containerized with Docker and deployed using Docker Compose. Nginx is used as a reverse proxy in front of the Angular frontend and Node/Express backend. CI/CD is implemented using GitHub Actions to build and push Docker images to Docker Hub on every push to main.

### Dockerized architecture

| Component | Image                               | Role                                                 |
| --------- | ----------------------------------- | ---------------------------------------------------- |
| MongoDB   | mongo:7                             | Stores tutorials collection.                         |
| Backend   | phanindhrasura/mean-backend:latest  | Node/Express REST API connected to Mongo.            |
| Frontend  | phanindhrasura/mean-frontend:latest | Angular 15 app built and served as static files.     |
| Nginx     | nginx:alpine                        | Reverse proxy routing/to frontend and/apito backend. |

### Nginx config (nginx/default.conf) routes traffic:

   server {
 
      listen 80;
    
      server_name _;
    
      location / {
    
          proxy_pass http://frontend:80;
        
      }
    
      location /api/ {
    
          proxy_pass http://backend:8080;
        
     }
    
  }


#### Backend connects to Mongo via the Docker network using environment variable:

 environment:
   `MONGO_URL=mongodb://mongo:27017/dd_db`

### Running with Docker Compose

##### Prerequisites: Docker and Docker Compose installed

From the project root:

    docker compose up -d 

    docker compose down 

Once running:

- Application UI: http://localhost (served by Nginx → Angular).

- API endpoint: http://localhost/api/tutorials (Nginx → backend).​

 Data in Mongo persists using the named volume mongo-data.

 ### CI/CD with GitHub Actions

 A GitHub Actions workflow at .github/workflows/docker-ci.yml automates Docker image builds and pushes to Docker Hub on each push to main.

 ##### Workflow steps:
- Checkout code (actions/checkout).

- Set up Docker Buildx for multi-platform builds.​

- Log in to Docker Hub using repo secrets:
  
  `DOCKERHUB_USERNAME`

  `DOCKERHUB_TOKEN` (Docker Hub access token with read/write).
  
- Build & push backend image from ./backend as:

  `phanindhrasura/mean-backend:latest`

  `phanindhrasura/mean-backend:<commit-sha>`

- Build & push frontend image from ./frontend with matching tags:

  `phanindhrasura/mean-frontend:latest`

  `phanindhrasura/mean-frontend:<commit-sha>` 
 
#### These images are then referenced by docker-compose.yml, so a fresh environment can run the app using only: 

    git clone https://github.com/Phanindhraaa/DOLLAR-ASSIGNMENT.git

    cd DOLLAR-ASSIGNMENT
 
    docker compose pull
 
    docker compose up -d


 



