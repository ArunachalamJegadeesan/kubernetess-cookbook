# kubernetess-cookbook
Kubernetees Hands on...!!!

### Workout

```
gcloud container clusters get-credentials standard-cluster-1 --zone us-central1-a --project kubernetes-01-222705

```


```
arun-mac:~ arunaja$ kubectl get pods
NAME                              READY     STATUS    RESTARTS   AGE
bff-fb65d7ffd-cgj6n               1/1       Running   0          10m
catalogservice-658db56b9b-nm5rv   1/1       Running   0          13m
frontend-7fc7b9b846-77hr8         1/1       Running   0          9m
arun-mac:~ arunaja$ kubectl get services
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
bff              ClusterIP      10.11.245.194   <none>          6060/TCP         10m
catalogservice   ClusterIP      10.11.244.246   <none>          7070/TCP         13m
frontend         LoadBalancer   10.11.249.62    35.232.64.189   8080:31745/TCP   10m
kubernetes       ClusterIP      10.11.240.1     <none>          443/TCP          41m
arun-mac:~ arunaja$
```
