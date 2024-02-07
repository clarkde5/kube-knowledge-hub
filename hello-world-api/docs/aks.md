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

   ```
   > TOKEN=$(az acr login --name todotestdocker --expose-token --output tsv --query accessToken)
   > docker login todotestdocker.azurecr.io -u 00000000-0000-0000-0000-000000000000 -p $TOKEN
   ```

3. Authenticate with AKS

   ```
   > az login
   > az aks get-credentials --resource-group sandbox --name dclark-aks
   ```
4. Build and push image to ACR using docker
   
   - Review [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)

   Build image to local docker registry
   ```
   > docker build . -t todotestdocker.azurecr.io/helloworldapi:latest
   ```

   Tag existing image for deployment
   ```
   > docker image tag helloworldapi:latest todotestdocker.azurecr.io/helloworldapi:latest
   ```

   Push image to ACR

   ```
   > docker push todotestdocker.azurecr.io/helloworldapi
   ```

5. AKS <-> ACR Connectivity (using secret)
   
   ```
   > kubectl create secret docker-registry todotestdocker-docker-registry-secret --docker-server=todotestdocker.azurecr.io -docker-username=00000000-0000-0000-0000-000000000000 --docker-password=$TOKEN --docker-email=none@none.com
   ```

6. Demo
    
    * Reference AKS interface and show correlation between CLI (kubectl)

7. [Networking concepts for applications in AKS](https://learn.microsoft.com/en-us/azure/aks/concepts-network)
8. [Ingress controller](https://learn.microsoft.com/en-us/azure/aks/concepts-network#ingress-controllers)
    
    - NGINX
    - Application Gateway Ingress Controller (AGIC)


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
