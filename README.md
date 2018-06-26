# my-kubectl-cheatsheet  

Collection of useful commands.  
</br>

- List of pods (more details: ip, nodes)

```
kubectl get pods -o wide
```

- Get the name of the Pod and store it in the POD_NAME environment variable:

```
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```  


- To apply a new label we use the label command followed by the object type, object name and the new label:

```
kubectl label pod $POD_NAME app=v1
```  
 
 
- Scale the Deployment to 4 replicas.  

```
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```


- Create an environment variable called NODE_PORT that has the value of the Node port assigned:

```
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```  


- To update the image of the application to version 2, use the set image command, followed by the deployment name and the new image version:

```
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```  


