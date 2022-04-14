# Описание
for kubernetes
конвертированно [Kompose](https://kompose.io/)

# Использование
для версии minikube
создать контейнеры
```bash
minikube image build -t hazelcast ./hazelcast
minikube image build -t elasticsearch ./elasticsearch
minikube image build -t cs ./cs
```

Применял последовательно
```bash
kubectl apply -f db-ConfigMap.yaml

kubectl apply -f db-deployment.yaml
kubectl apply -f db-service.yaml

kubectl apply -f hazelcast-deployment.yaml
kubectl apply -f hazelcast-service.yaml

kubectl apply -f elasticsearch-deployment.yaml
kubectl apply -f elasticsearch-service.yaml

kubectl apply -f cs-deployment.yaml
kubectl apply -f cs-service.yaml
```

