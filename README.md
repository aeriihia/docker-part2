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
      command: ["serve", "-s", "-l", "5000", "build"]
  backend:
      image: example-backend
      ports: 
        - 8080:8080
      environment:
        - REQUEST_ORIGIN=http://localhost:5000
      command: ./server
```