# Tech Stack

### Frontend
- Nuxt 3 ( Typescript )

### Backend
- FastAPI ( Python )

### Database
- PostgreSQL

### Caching 
- Redis

### Event Broker
- RabbitMQ - to be checked

### Containerization
- Docker images

# Architecture design

##### This is going to be monolith architecture as there is no need for multiple services where only one service involve here.

### DB Design

#### Discalaimer :
##### Since it is pretty straightforward we just need 1 database with no sharding.
##### Cache Invalidation : 10 minutes ( assuming 10 minutes acting like 3 days standard for medium large application )
##### Container Orchestration is not involved as this is for mini deomonstration only

```bash
read (cache aside):
               (return data and redis set)
              ----------------------------
              |                          |
              |                          |
              |                          |
Client -> Backend -> Redis -> if miss -> db
     ^------| ^         |                   
              |         | (hit redis)       
              |         |                   
              -----------
            (return data)

write (cache invalidation):
Client -> Backend -> db update
     ^------| ^         |                   
              |         | (hit redis)       
              |         |                   
              -----------
            (delete redis)
```

