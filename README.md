# docker-part2

## 2.1
docker-compose.yml
```
version: '3.5' 

services:
  simple-web-service: 
    image: devopsdockeruh/simple-web-service
    build: . 
    volumes: 
      - /home/text.log:/usr/src/app/text.log
```

## 2.2
docker-compose.yml
```
version: '3.5' 

services:
  simple-web-service: 
    image: devopsdockeruh/simple-web-service
    build: .
    ports: 
      - 8080:8080
    command: server
```

## 2.3
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000 
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:8080
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost:5000
```

## 2.4
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000 
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:8080
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost:5000
      - REDIS_HOST=redis
  redis:
    image: redis
```

## 2.5
Command
```
docker-compose up --scale compute=3
```

## 2.6
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000 
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:8080
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost:5000
      - REDIS_HOST=redis
      - POSTGRES_HOST=db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DATABASE=postgres
  redis:
    image: redis
  db:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_PASSWORD=postgres
```

## 2.7
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: ml-frontend
    ports: 
      - 3000:3000
  backend:
    image: ml-backend
    ports: 
      - 5000:5000
    volumes:
      - model:/src/model
  training:
    image: ml-training
    volumes:
      - model:/src/model
      - imgs:/src/imgs

volumes:
  model:
  imgs:
```

## 2.8
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000 
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:8080
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost:5000
      - REDIS_HOST=redis
      - POSTGRES_HOST=db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DATABASE=postgres
  redis:
    image: redis
  db:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_PASSWORD=postgres
  proxy:
    image: nginx
    ports:
      - 80:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
```

## 2.9
docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000 
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:8080
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost:5000
      - REDIS_HOST=redis
      - POSTGRES_HOST=db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DATABASE=postgres
  redis:
    image: redis
  db:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - database:/var/lib/postgresql/data
  proxy:
    image: nginx
    ports:
      - 80:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf

volumes:
  database:
```

## 2.10
I removed REACT_APP_BACKEND_URL=http://localhost:8080 and changed REQUEST_ORIGIN=http://localhost:5000 to REQUEST_ORIGIN=http://localhost.

docker-compose.yml
```
version: '3.5'

services:
  frontend:
    image: example-frontend
    ports: 
      - 5000:5000
  backend:
    image: example-backend
    ports: 
      - 8080:8080
    environment:
      - REQUEST_ORIGIN=http://localhost
      - REDIS_HOST=redis
      - POSTGRES_HOST=db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DATABASE=postgres
  redis:
    image: redis
  db:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - database:/var/lib/postgresql/data
  proxy:
    image: nginx
    ports:
      - 80:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf

volumes:
  database:
```

Dockerfile (Frontend)
```
FROM node
EXPOSE 5000
WORKDIR /usr/src/app
COPY . .
RUN npm install
RUN npm run build
RUN npm install -g serve
CMD ["serve", "-s", "-l", "5000", "build"]
```

Dockerfile (Backend)
```
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build
CMD ./server
```