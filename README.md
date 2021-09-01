# docker-part2

## 2.1
docker-compose.yml
```
version: '3' 

services: 

    simple-web-service: 
      image: devopsdockeruh/simple-web-service
      build: . 
      volumes: 
        - /home/text.log:/usr/src/app/text.log
```