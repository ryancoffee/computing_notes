## Building on LDRD cluster  
```bash
coffee@pskube-gru ~ $ kubectl get pods
coffee@pskube-gru ~ $ kubectl delete -f ./kubernetes/devtools7coffee.yaml
coffee@pskube-lucy docker $ docker images
coffee@pskube-lucy docker $ docker build --rm -t pshub01:5000/devtools7coffee -f Dockerfile.devtools7coffee .
coffee@pskube-lucy docker $ docker push pshub01:5000/devtools7coffee:latest
```

## Deploying on LDRD cluster  
```bash
coffee@pskube-gru ~ $ kubectl get pods
coffee@pskube-gru ~ $ kubectl create -f ./kubernetes/devtools7coffee.yaml
coffee@pskube-gru ~ $ kubectl get pods
coffee@pskube-gru ~ $ kubectl exec -ti devtools7coffee -- /bin/bash
```
