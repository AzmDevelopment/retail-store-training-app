# Retail sample app

This is a sample application designed to illustrate various concepts related to containers. It presents a sample retail store application including a product catalog, shopping cart and checkout.

It provides:

- A distributed component architecture in various languages and frameworks
- Utilization of a variety of different persistence backends for different components like MariaDB (or MySQL), DynamoDB and Redis
- The ability to run in various container orchestration technologies like Docker Compose, Kubernetes etc.
- All components instrumented for Prometheus metrics and OpenTelemetry OTLP tracing
- Support for Istio on Kubernetes
- Load generator which exercises all of the infrastructure

![Screenshot](/docs/images/screenshot.png)

## Application Architecture

The application has been deliberately over-engineered to generate multiple de-coupled components. These components generally have different infrastructure dependencies, and may support multiple "backends" (example: Carts service supports MongoDB or DynamoDB).

![Architecture](/docs/images/architecture.png)

| Component                                                                                                                   | Language | Container Image                                                             | Description                                                                 |
| --------------------------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| UI | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-ui)       | Aggregates API calls to the various other services and renders the HTML UI. |
| Catalog   | Go       | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-catalog)  | Product catalog API                                                         |
| Cart         | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-cart)     | User shopping carts API                                                     |
| Orders     | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-orders)   | User orders API                                                             |
| Checkout | Node     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-checkout) | API to orchestrate the checkout process                                     |
| Assets     | Nginx    | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-assets)   | Serves static assets like images related to the product catalog             |

## Quickstart

The following sections provide quickstart instructions for various platforms. All of these assume that you have cloned this repository locally and are using a CLI thats current directory is the root of the code repository.

### Docker Compose

This deployment method will run the application on your local machine using `docker-compose`, and will build the containers as part of the deployment.

Pre-requisites:

- Docker installed locally

Use `docker compose` to run the application containers:

```
MYSQL_PASSWORD='<some password>' docker compose --file docker-compose.yml up
```

Open the frontend in a browser window:

```
http://localhost:8888
```

To stop the containers in `docker compose` use Ctrl+C. To delete all the containers and related resources run:

```
docker compose -f docker-compose.yml down
```

## Project

This is a microservice application and most dockerfiles have been already provided to you. As always fork this repo and create git branches as necessary. Do not push to main.

### Tasks
- Containerization
    - Create Dockerfile for checkout service.
    - Create Dockerfile for catalog service.
    - Create Dockerfile for assets service.
    - Replace all image fields in docker-compose.yml with build fields with appropriate context and dockerfile. The goal is so that compose is no longer pulling images from ECR but building all of them locally.
- Create CI pipelines on github actions.
    - When there is a push to main branch for a service, you have to build it's docker image and push to your dockerhub.
    - Do the same when there is a pull request with main as the base branch and changes is a specific service.
- Create kubernetes manifests and deploy complete application in minikube.
- Package the application using helm chart.