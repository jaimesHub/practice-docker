# Docker compose: Using Docker Compose With Dockerfile to Deploy Application

## Index
```
1. Install Docker Compose
2. Create Compose File
3. Run Docker Compose
```

## References
```
https://github.com/dockersamples/example-voting-app
```
## Practicing

`Step 0:` cd /practice/demo-2

`Step 1:` Creating Docker Compose file
```
cat > docker-compose.yml
redis:

db:

vote:

worker:

result:
```

`Step 2:` Create `docker-compose.yml` file
```
sudo nano/vi docker-compose.yml
```

```
redis:
  image: redis
db:
  image: postgres:15-alpine
vote:
  image: voting-app
  ports:
    - 5000:80
  links:
    - redis
worker:
  image: worker-app
  links:
    - db
    - redis
result:
  image: result-app
  ports:
    - 5001:80
  links:
    - db
```

`Step 2.1`: Checking version 3
```
https://docs.docker.com/compose/compose-file/compose-file-v3/
```
```
version: '3'
services:
  redis:
    image: redis
  db:
    image: postgres:15-alpine
    environment:
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: postgres
  vote:
    image: voting-app
    ports:
      - 5000:80
  worker:
    image: worker-app
  result:
    image: result-app
    ports:
      - 5001:80
```

`Step 3:` Run Docker Compose
```
docker-compose up
```
