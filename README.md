# MEAN Stack DevOps Deployment | Discover Dollar Assignment

This project demonstrates complete DevOps implementation by containerizing and deploying a full-stack **MEAN (MongoDB, Express, Angular, Node.js)** CRUD application using:

- Docker & Docker Compose
- AWS EC2
- Nginx Reverse Proxy
- GitHub Actions CI/CD Pipeline
- Docker Hub Image Registry
- MongoDB container for database

---

#  Tech Stack
| Component | Technology Used |
|-----------|----------------|
Frontend | Angular 15 + Nginx |
Backend | Node.js + Express |
Database | MongoDB Docker image |
Infrastructure | AWS EC2  |
Containerization | Docker, Docker Hub |
Reverse Proxy | Nginx |
CI/CD | GitHub Actions |

---

#  Project Steps
## STEP 1:Containerize Backend (Node.js / Express)
###  Create `Dockerfile` inside `/backend`
```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### Build & Push Backend Image to Docker Hub
command for build:
```
docker build -t pratheek1807/backend:latest backend
```
command for push:
```
docker push pratheek1807/backend:latest
```

## STEP 2:Containerize Frontend (Angular + Nginx)
### Create Dockerfile inside /frontend
```
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --configuration=production
FROM nginx:alpine
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html
EXPOSE 80
```

### Build & Push Frontend Image
command for build:
```
docker build -t pratheek1807/frontend:latest frontend
```
command for push:
```
docker push pratheek1807/frontend:latest
```


## STEP 3:Deploy Application using Docker Compose on AWS EC2

### Create docker-compose.yml in project root
* create a docker compose file

### Run Deployment
```
 docker-compose up -d
 docker ps
```


## STEP 4 : Configure Nginx Reverse Proxy 

### Install NGINX
```
 sudo apt install nginx -y
```

### Edit default configuration 
```
  server {
    listen 80;

    location / {
        proxy_pass http://localhost:80;
    }

    location /api/ {
        proxy_pass http://localhost:3000/;
    }
}
```

### Restart NGINX and Access application
  http:// ip_addr



## STEP 5 :Configure CI/CD using GitHub Actions

### Create .github/workflows/deploy.yml
* add repo secrets
 ```
   -> EC2_HOST Elastic = IP
   -> EC2_USERNAME = ubuntu
   -> EC2_KEY	private = SSH key
   -> DOCKER_USERNAME	= Docker Hub username
   -> DOCKER_PASSWORD	= Docker Hub token/password
```
* commit the changes
* pipeline starts running we can check for logs by click on git hub action

### Final URL (Live Application)
*  http:// ip_addr--> access the app










