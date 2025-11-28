This project is a full-stack CRUD application built with the MEAN stack (MongoDB, Express, Angular 15, Node.js). It manages a collection of tutorials with fields: id, title, description, and published. Users can create, retrieve, update, delete, and search tutorials by title.​

The application is fully containerized with Docker and deployed using Docker Compose. Nginx is used as a reverse proxy in front of the Angular frontend and Node/Express backend. CI/CD is implemented using GitHub Actions to build and push Docker images to Docker Hub on every push to main.

## Local development (without Docker)
### Backend (Node.js + Express)
`cd backend`

`npm install`

`node server.js`

The backend exposes REST APIs under http://localhost:8080/api/tutorials.
### Frontend (Angular 15)
`cd frontend`

`npm install`

`ng serve --port 8081`

Angular will run on `http://localhost:8081/` and uses HttpClient to call the backend. In local (non‑Docker) mode, the base URL is configured in `src/app/services/tutorial.service.ts`.
