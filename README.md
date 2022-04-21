# Todo Application: Angular Frontend + ASP.NET Core API + SQL Database

## Pre-requisites

1.  Create new Azrue SQL Database
2.  Restore the contents of the database using the SQL script in the database folder
3.  Create new Azure Container Registry
4.  Create new AKS Cluster

## Run the application using Docker

1.  Build todo-api docker image

        docker build -t todo-api:v1 .

2.  Build todo-web docker image

        docker build -t todo-web:v1 .

3.  Check the new images

        docker images

4.  Run the todo-api as container

        # Interactive mode
        docker run -it --rm -p 8081:80 --env "ConnectionStrings:SQLConnection=<Database Connection String>" -e "ASPNETCORE_ENVIRONMENT=Development" todo-api:v1

        # Detached mode
        docker run -d -p 8081:80 --env "ConnectionStrings:SQLConnection=<Database Connection String>" -e "ASPNETCORE_ENVIRONMENT=Development" todo-api:v1

5.  Run the todo-web app as container

        # Interactive mode
        docker run -it --rm -p 8080:80 --env API_URL="http://localhost:8081" todo-web:v1

        # Detached mode
        docker run -d -p 8080:80 --env API_URL="http://localhost:8081" todo-web:v1

6.  Access the Todo web app at http://localhost:8080

## Deploy the appliation using Azure Kubernetes Service (AKS)

1.  Tag the docker images

        # Tag the images
        docker tag todo-api:v1 [ACR URL]/todo-api:v1
        docker tag todo-web:v1 [ACR URL]/todo-web:v1

2.  Push the images into ACR

        # Log into into Azure Container Registry
        az acr login --name [ACR Name]

        # Push the images into ACR
        docker push [ACR URL]/todo-api:v1
        docker push [ACR URL]/todo-web:v1

3.  Go to ACR and check the new images

4.  Create the todo namespace

        kubectl create ns todo-ns

5.  Deploy the todo-api

        kubectl apply -f todo-api-dep.yaml

6.  Showcase the API running through command line and Azure portal

7.  Deploy the todo-api-service

        kubectl apply -f todo-api-service.yaml

8.  Replace the IP address of the ingress controller in the host

9.  Deploy the ingress route for todo-api

        kubectl apply -f todo-api-ingress.yaml

10. Repeat steps 5 to 9 for todo-web

        kubectl apply -f todo-web-dep.yaml

        kubectl apply -f todo-web-service.yaml

        kubectl apply -f todo-web-ingress.yaml
