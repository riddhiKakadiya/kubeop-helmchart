## go to the operator folder and run the following commands	
```bash	
operator-sdk generate k8s && operator-sdk generate openapi

operator-sdk build {{docker username}}/team2-kubeop:latest

docker push {{docker username}}/team2-kubeop:latest
```

## setting up helm
```bash
kubectl create -f helm-setup/rbac-config.yaml
helm init --service-account tiller
```

## Installing operator 
```bash
kubectl create secret docker-registry regcred \
	--docker-server=docker.io \
	--docker_username= \
	--docker_password= \
	--docker_email= \
```

```bash
helm install --name {{release-name}} s3folder-kubeop-chart --set image.repository={{docker username}}/team2-kubeop --set image.tag=latest
```

## go into the operator folder and create the cr  
```bash
kubectl apply -f helm-setup/cr.yaml
```

## delete release 
```bash
helm del --purge {{release-name}}
```