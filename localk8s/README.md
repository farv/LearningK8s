# Kubernetes from a localhost cluster. My annotations:

*Here is the Cheatsheet for ***Kubectl***!* -> [https://kubernetes.io/docs/reference/kubectl/cheatsheet/]

## Create a namespace called 'learningkubernetes' and a pod with image nginx called nginx on this namespace
``` bash
    kubectl create namespace learningkubernetes
    kubectl create deployment nginx --image=nginx --namespace=learningkubernetes
```
## From 0 to "Welcome to Nginx" in localhost
``` sh
    kubectl create namespace learningkubernetes
    kubectl create deployment ${deployment_name} --image=nginx:latest --namespace=learningkubernetes
    kubectl expose deployment ${deployment_name} --type=NodePort --port=80 --namespace=learningkubernetes
    PORT=$(kubectl get svc ${deployment_name} -n learningkubernetes -o jsonpath='{.spec.ports[0].nodePort}') && curl localhost:$PORT
```
Or visit the URL **localhost:${PORT}**, changing that PORT variable for the response obtained in:
``` sh
    kubectl get svc nginx -n learningkubernetes -o jsonpath='{.spec.ports[0].nodePort}')
```
# Core Concepts:

## Get a Shell to a Running Container:

https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/

### Sumary

+ To use this [file](nginx.yaml) just execute `kubectl apply -f <filePath>`.
+ To get information from the previous deployment `kubectl get pod nginx`.
+ To access the shell of the service `kubectl exec --stdin --tty nginx -- /bin/bash`. Exec is used to execute a command inside the container, as it is `docker exec ...`
+ BONUS TRACK: When a pod has 2 containers you may declare the container with `--container <containerName>`

## Configure Access to Multiple Clusters

https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/

### Sumary

+ Cluster, users AND **contexts**.
+ KUBECONFIG as a env variable for more config files.

## Accessing Clusters

https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/

## Sumary

You can do the same from cli or from API or from a client library, you have Python and Go, for instance. To obtain the API URL:
+ From PROXY:
    ``` sh
    kubectl proxy --port=8080
    curl localhost:8080/api/v1/namespaces # Similar to kubectl get namespaces
    ```
+ Directly from master. To this we have to obtain the access token and add it to the request as `Authentication: Bearer`
    ``` sh
    kubectl cluster-info # gets URLs
    SECRET_NAME=$(kubectl get serviceaccount default -o jsonpath='{.secrets[0].name}') # obtain secret name
    kubectl get secret $SECRET_NAME -o jsonpath='{.data.token}' | base64 --decode # obtain access token
    ```
+ Client library as Go, Python, Haskell (o.O)
+ Access from the **pod**. Inside the pod there is a folder `/var/run/secrets/kubernetes.io/serviceaccount` where we
can find text files with useful content. Token and ca.crt are useful for an API request like:
    ``` sh
    curl --cacert ${CACERT} -X GET https://kubernetes.default.svc/api/v1/namespaces -header "Authorization: Bearer $(echo $TOKEN)"
    ```

## Network policy



## Use Port Forwarding to Access Applications in a Cluster

https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/

## Sumary

