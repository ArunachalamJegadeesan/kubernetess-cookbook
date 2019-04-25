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

# _Looking deep into the pod and understanding the environment varibales generated_

```
arun-mac:~ arunaja$ kubectl exec -it frontend-7fc7b9b846-77hr8  /bin/sh
/deploy # ls -ltr
total 22244
-rw-r--r--    1 root     root      22776097 Apr 24 04:08 productCatalogWeb-1.5.8.RELEASE.war
/deploy # printenv
FRONTEND_PORT_8080_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.11.240.1:443
FRONTEND_PORT=tcp://10.11.249.62:8080
FRONTEND_SERVICE_PORT=8080
JAVA_ALPINE_VERSION=8.201.08-r1
HOSTNAME=frontend-7fc7b9b846-77hr8
BFF_SERVICE_PORT=6060
BFF_PORT=tcp://10.11.245.194:6060
SHLVL=1
BFF_PORT_6060_TCP=tcp://10.11.245.194:6060
HOME=/root
bff.endpoint.url=http://10.11.245.194:6060/bff/catalog
FRONTEND_PORT_8080_TCP=tcp://10.11.249.62:8080
SUFIX=/bff/catalog
JAVA_VERSION=8u201
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.11.240.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin
KUBERNETES_PORT_443_TCP_PORT=443
CATALOGSERVICE_PORT_7070_TCP_ADDR=10.11.244.246
KUBERNETES_PORT_443_TCP_PROTO=tcp
LANG=C.UTF-8
CATALOGSERVICE_SERVICE_HOST=10.11.244.246
CATALOGSERVICE_PORT_7070_TCP_PORT=7070
CATALOGSERVICE_PORT_7070_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.11.240.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
SEPERATOR=:
CATALOGSERVICE_SERVICE_PORT=7070
CATALOGSERVICE_PORT=tcp://10.11.244.246:7070
KUBERNETES_SERVICE_HOST=10.11.240.1
BFF_PORT_6060_TCP_ADDR=10.11.245.194
JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
PWD=/deploy
PROTOCOL=http://
CATALOGSERVICE_PORT_7070_TCP=tcp://10.11.244.246:7070
FRONTEND_PORT_8080_TCP_ADDR=10.11.249.62
FRONTEND_SERVICE_HOST=10.11.249.62
BFF_PORT_6060_TCP_PORT=6060
BFF_PORT_6060_TCP_PROTO=tcp
BFF_SERVICE_HOST=10.11.245.194
FRONTEND_PORT_8080_TCP_PORT=8080
/deploy #
```
