# Docker compose: Using Docker Run Commands With Dockerfile to Deploy Application

## Theories
- we first learned how to run a Docker container using the `Docker run command`.

- If we needed to set up a complex application running multiple services, a better way to do it is to use `Docker Compose`.

- docker-compose.yml
    ```
        - create a configuration file in YAML format and put together 
        - put together different services and the options specific
    ```

- running this file -> `docker-compose up`: to bring up the entire application stack

- Sample application to demonstrate Docker Compose - `Voting application`
    ```
    Architecture & Data flow
    ```
    ![Architecture diagram](architecture.excalidraw.png)

## Practicing
1. Repo

    ```
    https://github.com/dockersamples/example-voting-app
    ```

2. Steps

    `Step 1:` Building the docker images by running below command
    ```
    docker build {{commands}}
    ```

    `Step 2:` demo-1
    ```
    Checking resource on practice/demo-1
    ```

    `Step 3:` Running the `vote application` as `Client side - Voting page`
    ```
    #0 Checking example-voting-app/vote
    #1 Build docker image: 
        docker build . -t voting-app
    #2 Run image to create container: 
        docker run -p 5000:80 voting-app
    #3 Problems: While trying to vote "Dog|Cat" => Internal Server Error
    #4 Stop voting-app container to create redis container first
    ```

    `Step 4`: Running `redis` container as `a Database instance`
    ```
    #1 Start a Redis image in detach mode: 
        docker run -d --name=redis redis
    #2 Start a voting-app image instance that maps to redis: 
        docker run -d -p 5000:80 --link redis:redis voting-app
    #3 test voting "Dog|Cat" with an built-interface http://localhost:5000
    ```

    `Step 5:` Deploy a `postgresql` database instance
    ```
    # docker run -d --name=db postgres:15-alpine
    ```

    `Step 6:` Deploy a `worker application` as `a Back-end Service`
    ```
    #1 Checking example-voting-app/worker
    #2 Create and build an image: 
        docker build . -t worker-app
    #3 Create a container and map it to db and redis instance: 
        docker run -d --link redis:redis --link db:db worker-app
    ```

    `Step 7:` Create an image for the `result application` as `a Client Side - Result Page`
    ```
    #1 Checking example-voting-app/result
    #2 Create an image: 
        docker build . -t result-app
    #3 Create + Build + Running a container and map it to db instance:
        docker run -d -p 5001:80 --link db:db result-app
    ```

    `Step 8:` Testing

## Recap
    ```
    - Understanding the architecture of an application & How it works
    - Deploying it with docker run commands using --links parameter
    ```
