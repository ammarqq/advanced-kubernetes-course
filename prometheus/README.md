# installation
From: https://github.com/coreos/prometheus-operator

rbac:
```
kubectl create -f rbac.yml
```

prometheus:
```
kubectl create -f prometheus.yml
kubectl create -f prometheus-resource.yml
```

Kubernetes monitoring:
```
kubectl create -f kubernetes-monitoring.yml
```

Example app:
```
kubectl create -f example-application.yml
```
kubectl get prometheus
kubectl get statefulset
kubectl get svc
kubectl get svc/prometheus

a7b6b3dc3bdb311e880ab0a64c92e7c1-140901031.eu-west-1.elb.amazonaws.com:9090

status--target
