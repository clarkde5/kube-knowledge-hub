# Environment Setup

## Dependencies

1. Create or use existing Azure Kubernetes Services (AKS)
2. Create or use existing Azure Container Registry (ACR)

## Review

1. Key factors for resource expenses within AKS
    
    1. Node pools
    2. Node size
    3. Max pods per Node

2. Authenticate with ACR
3. Authenticate with AKS
4. Build and Publish image to ACR using docker
5. AKS <-> ACR Connectivity (using secret)
6. Demo
    
    * Reference AKS interface and show correlation between CLI (kubectl)

7. Networking models
8. Ingress controller


# Demo

## Create pod

```
(term)> kubectl apply -f aks/pod.yml
```

## Port-Forward to Pod

```
(term)> kubectl port-forward pods/helloworldapi 5000:5000
```

## Create LoadBalancer Service

```
(term)> kubectl apply -f aks/service.yml

(web)> http://<ip>:8080/WeatherForecast
```

## Delete pod

```
(term)> kubectl delete pods/helloworldapi
```

## Create Deployment

```
(term)> kubectl apply -f aks/deployment.yml
```

## Delete pod again

```
(term)> kubectl delete pod -l app=helloworldapi
(term)> kubectl get pods
```

## Edit deployment to increse replicas

```
(term)> export KUBE_EDITOR='code --wait'
(term)> kubectl edit deployment/helloworldapi-deployment
```

## Cleanup

```
kubectl delete deployment/helloworldapi-deployment 
kubectl delete service/helloworldapi-load-balancer-service
```
