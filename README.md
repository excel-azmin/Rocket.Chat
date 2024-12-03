# Rocket.Chat

```
version: '3.8'

volumes:
  mongodb_data: { driver: local }

services:
  mongodb:
    image: docker.io/bitnami/mongodb:6.0
    container_name: rocketchat-mongodb
    restart: always
    volumes:
      - mongodb_data:/bitnami/mongodb
    environment:
      MONGODB_REPLICA_SET_MODE: primary
      MONGODB_REPLICA_SET_NAME: rs0
      MONGODB_PORT_NUMBER: 27017
      MONGODB_DATABASE: rocketchat
      ALLOW_EMPTY_PASSWORD: ${ALLOW_EMPTY_PASSWORD}

  rocketchat:
    image: registry.rocket.chat/rocketchat/rocket.chat:latest
    container_name: rocketchat
    restart: always
    environment:
      MONGO_URL: mongodb://mongodb:27017/rocketchat?replicaSet=rs0
      MONGO_OPLOG_URL: mongodb://mongodb:27017/local?replicaSet=rs0
      ROOT_URL: http://0.0.0.0:3000
      PORT: 3000
    depends_on:
      - mongodb
    ports:
      - "3000:3000"



```


* DB Configuration

```
docker exec -it rocketchat-mongodb mongosh
rs.initiate()
rs.status()
```
  
