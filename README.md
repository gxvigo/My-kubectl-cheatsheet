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


- To update the image of the application to version 2, use the set image command, followed by the deployment name and the new image version. This will not only set the new image, but it triggers the updates of the pods.

```
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```  
  
- Cleanup inactive pods

```
txt="0/";ns="ms-demo";type="pods"; kubectl -n $ns get $type | grep "$txt" | awk '{ print $1 }' | xargs kubectl -n $ns delete $type
```

- Get a shell into a running container  (check that the pod exist and it's running):  

```
kubectl get pod vf-meetingapi-5b7595c5db-7mvmx -n ms-demo
```  

```
kubectl -n ms-demo exec -it vf-meetingapi-5b7595c5db-7mvmx -- /bin/sh
```


- How to build *specific* kubectl commands: [TO-DO]

All Kubernetes resource state is accessible through e.g. kubectl get (in my experience more useful for this purpose than kubectl describe) commands. Then, all that’s left is to find the needle in what can be a haystack of JSON.

The trick is to:

```
kubectl get [resource]/[resource-name] --output=JSON
```  
And then eyeball the results to start building a query string:

```  
kubectl get [resource]/[resource-name] --output=jsonpath=".items[*]"
```

and iteratively refine the result set down until you have the item(s) that you seek. Here’s an example that should work with any cluster:

```
kubectl get nodes --output=json
kubectl get nodes --output=jsonpath="{.items[*]}
kubectl get nodes --output=jsonpath="{.items[0]}
kubectl get nodes --output=jsonpath="{.items[0].metadata.name}
``` 
Lastly, there’s a decent argument (and a tenet in *nix) for learning one JSON parsing tool and applying that tool to all JSON parsing needs. In this case, is there any reasonable competitor to jq? I suspect not.

Plus jq has an excellent playground (jqplay.org).


