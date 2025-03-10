# Weather App

This project is a weather service system composed of multiple microservices for collecting weather data, publishing it to Kafka, processing weather data for notifications, and managing communication via Kafka and Zookeeper. The system is managed using Helm charts and deployed via ArgoCD.

## Repositories

### 1. **weather-service**
This service is responsible for fetching weather data from an external weather API and publishing it to a Kafka topic for further processing by the weather notifications service.

#### Features:
- **/weather**: Endpoint that fetches and shows current weather data.
- **/publish-weather**: Endpoint that publishes the weather data to Kafka.

### 2. **weather-notifications-service**
This service listens to the Kafka topic where weather data is published and processes it. It provides weather alerts based on the weather data (e.g., alerts when it’s raining).

#### Features:
- **/weather**: Returns current temperature and weather conditions (clear/rainy).
- **/alerts**: Endpoint that triggers alerts if it’s raining.

### 3. **weather-infra**
Contains the necessary infrastructure components, including Kafka and Zookeeper, that enable communication between the weather services.

### 4. **weather-bootstrap**
Contains the bootstrap files for setting up ArgoCD to manage the deployment and lifecycle of the services on Kubernetes.

## Architecture Overview

- The **weather-service** fetches weather data from an external API and publishes it to Kafka.
- The **weather-notifications-service** listens for weather data in Kafka, processes it, and generates notifications if necessary (e.g., alerts when it is raining).
- **weather-infra** handles the communication backbone with Kafka and Zookeeper.
- **weather-bootstrap** contains the ArgoCD configurations to automate deployments and manage Helm charts for the entire stack.

## Deployment

All services are deployed to a Kubernetes cluster (e.g., AWS EKS) using Helm charts and ArgoCD for continuous deployment.

### Prerequisites

- Kubernetes cluster (AWS EKS or other)
- Helm
- ArgoCD

### Installation

1. Clone the repositories:
    ```bash
    git clone https://github.com/your-org/weather-service
    git clone https://github.com/your-org/weather-notifications-service
    git clone https://github.com/your-org/weather-infra
    git clone https://github.com/your-org/weather-bootstrap
    ```

2. Deploy the infrastructure using ArgoCD and Helm:
    - Make sure that ArgoCD is connected to your Kubernetes cluster and the `weather-bootstrap` repository is synced.
    - Use Helm charts to deploy the services and Kafka infrastructure.

3. Access the services:
    - **weather-service**: Exposes APIs for fetching and publishing weather data.
    - **weather-notifications-service**: Exposes APIs for checking the current weather and receiving alerts.

### Cleanup

To remove the services and infrastructure from the Kubernetes cluster, you can run the following command via ArgoCD:
```bash
argocd app delete weather-service --cascade
argocd app delete weather-notifications-service --cascade
argocd app delete weather-infra --cascade
