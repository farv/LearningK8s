* Create a namespace called 'learningkubernetes' and a pod with image nginx called nginx on this namespace

```kubectl create namespace learningkubernetes```
```kubectl run nginx --image=nginx --restart=Never --namespace=learningkubernetes```