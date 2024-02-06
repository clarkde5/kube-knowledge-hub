# Environment Setup

## Reference
- https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu-2204
- https://stackoverflow.com/questions/77498786/unable-to-locate-package-dotnet-sdk-8-0

```
> lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
```

# Project

Project created using [quickstart-aspnet-core](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core)

# Demo

## Deploy App to Docker

```
(term)> dotnet build

(vscode)> Ctrl+Shift+P, Docker Images: Build Image

(vscode)> F5

(web)> http://localhost:5000/swagger/index.html
(web)> http://localhost:5000/WeatherForecast
```

## Create pod

```
(term)> kubectl apply -f k8s/pod.yml
```

## Port-Forward to Pod

```
(term)> kubectl port-forward pods/helloworldapi 5000:5000
```

## Create LoadBalancer Service

```
(term)> kubectl apply -f k8s/service.yml

(web)> http://localhost:8080/WeatherForecast
```

## Delete pod

```
(term)> kubectl delete pods/helloworldapi
```

## Create Deployment

```
(term)> kubectl apply -f k8s/deployment.yml
```

## Delete pod again

(term)> kubectl delete pod -l app=helloworldapi
(term)> kubectl get pods

## Edit deployment to increse replicas

(term)> export KUBE_EDITOR='code --wait'
(term)> kubectl edit deployment/helloworldapi-deployment

## Cleanup

kubectl delete deployment/helloworldapi-deployment 
kubectl delete service/helloworldapi-load-balancer-service
