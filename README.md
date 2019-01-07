
# From scratch with new case endpoint  /case
## Run Docker in minitube, so that the images are built and pulled from there
 eval $(minikube docker-env)

# build the image
 build -t rest-api .

# deploy image, pod/deployment rest-api  --image-pull-policy=Never, stops it trying to pull from dockerHub
 kubectl run rest-api --image=rest-api:latest --image-pull-policy=Never

# Check it's up and running
 kubectl get pods

 C02S50ELG8WM:rest-api lukeloze$ kubectl get pods
  NAME                          READY   STATUS    RESTARTS   AGE
  hello-foo-d88d775fc-2vqnd     1/1     Running   0          1h
  hello-node-7f5b6bd6b8-nwbmz   1/1     Running   0          5h
  rest-api-dd89fd864-xdn4g      1/1     Running   0          50s
  C02S50ELG8WM:rest-api lukeloze$ kubectl get deployments
  NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
  hello-foo    1         1         1            1           1h
  hello-node   1         1         1            1           5h
  rest-api     1         1         1            1           1m

 # Make a Service to expose this 
 kubectl expose deployment rest-api --type=LoadBalancer --port=8003

# At present I use the: 
# Gets the IP address, then take it and stick /case on the end.
minikube service rest-api

###############################################################################

# Doing it from yaml, @current this is without a service 
kubectl create -f deployment.yaml

# Now add service layer
kubectl expose deployment rest-api-deployment --type=LoadBalancer --port=8005


#kubectl create -f deployment.yaml

#kubectl expose deployment rest-api --type=LoadBalancer --port=8000

#worked
#kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never

#  C02S50ELG8WM:rest-api lukeloze$ kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never
#  kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
#  deployment.apps/hello-foo created
#  C02S50ELG8WM:rest-api lukeloze$ kubectl get pods
#  NAME                          READY   STATUS    RESTARTS   AGE
#  hello-foo-d88d775fc-2vqnd     1/1     Running   0          14s
#  hello-node-7f5b6bd6b8-nwbmz   1/1     Running   0          3h
#  C02S50ELG8WM:rest-api lukeloze$ kubectl expose deployment rest-api-deployment --type=LoadBalancer --port=8005
#Error from server (NotFound): deployments.extensions "rest-api" not found
#  C02S50ELG8WM:rest-api lukeloze$
#  C02S50ELG8WM:rest-api lukeloze$ kubectl get deployments
#  NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
#  hello-foo    1         1         1            1           17m
#  hello-node   1         1         1            1           3h
#  C02S50ELG8WM:rest-api lukeloze$ kubectl expose deployment hello-foo --type=LoadBalancer --port=8005
#  service/hello-foo exposed

# Then ran: minikube service hello-foo, this then


kubectl expose deployment rest-api-deployment --type=LoadBalancer --port=8005

