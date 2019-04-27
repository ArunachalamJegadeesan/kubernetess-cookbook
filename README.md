# kubernetess-cookbook
Kubernetees Hands on...!!!

 #### Working version of multitier kubernetes web , microservices and database application with persistance volume 


### Workout
#### Follow the order of apply because of interdependendcy of pods

```
gcloud container clusters get-credentials standard-cluster-1 --zone us-central1-a --project kubernetes-01-222705

kubectl create -f mysql-deployment.yaml

kubectl apply -f backend-deployment.yaml
deployment.apps/catalogservice created
service/catalogservice created

kubectl apply -f middletier-deployment.yaml
deployment.apps/bff created
service/bff created

kubectl apply -f frontend-deployment.yaml
deployment.apps/frontend created
service/frontend created

```


```
arun-mac:kubernetess-cookbook arunaja$ kubectl get pods
NAME                              READY     STATUS    RESTARTS   AGE
bff-fb65d7ffd-mdjhv               1/1       Running   0          14m
catalogservice-78b9749899-hgml5   1/1       Running   0          6m
catalogservice-78b9749899-sfrq2   1/1       Running   0          7m
frontend-7fc7b9b846-2svrx         1/1       Running   0          14m
frontend-7fc7b9b846-xghd9         1/1       Running   0          14m
mysql-5bc986964c-dng6x            1/1       Running   0          18m
arun-mac:kubernetess-cookbook arunaja$ kubectl get services
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
bff              ClusterIP      10.11.253.223   <none>           6060/TCP         14m
catalogservice   ClusterIP      10.11.240.224   <none>           7070/TCP         14m
frontend         LoadBalancer   10.11.246.122   35.226.106.229   8080:32729/TCP   14m
kubernetes       ClusterIP      10.11.240.1     <none>           443/TCP          21m
mysql            ClusterIP      10.11.253.1     <none>           3306/TCP         18m
arun-mac:kubernetess-cookbook arunaja$

```

## _Looking in to the mysql database POD and querying the database 


```

arun-mac:kubernetess-cookbook arunaja$ kubectl exec -it mysql-5bc986964c-dng6x /bin/sh
# printenv
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.11.240.1:443
HOSTNAME=mysql-5bc986964c-dng6x
MYSQL_MAJOR=5.6
HOME=/root
MYSQL_ROOT_PASSWORD=root
TERM=xterm
MYSQL_PORT_3306_TCP_ADDR=10.11.253.1
KUBERNETES_PORT_443_TCP_ADDR=10.11.240.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MYSQL_VERSION=5.6.44-1debian9
KUBERNETES_PORT_443_TCP_PORT=443
MYSQL_PORT_3306_TCP_PORT=3306
KUBERNETES_PORT_443_TCP_PROTO=tcp
MYSQL_SERVICE_HOST=10.11.253.1
MYSQL_PORT_3306_TCP_PROTO=tcp
MYSQL_SERVICE_PORT=3306
MYSQL_PORT=tcp://10.11.253.1:3306
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.11.240.1:443
MYSQL_PORT_3306_TCP=tcp://10.11.253.1:3306
GOSU_VERSION=1.7
KUBERNETES_SERVICE_HOST=10.11.240.1
PWD=/
# mysql -uroot -proot
Warning: Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 21
Server version: 5.6.44 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+---------------------+
| Database            |
+---------------------+
| information_schema  |
| #mysql50#lost+found |
| mysql               |
| performance_schema  |
| test                |
+---------------------+
5 rows in set (0.00 sec)

mysql> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select * from products;
+----+-----------+-----------------+-------------+------------+-------+
| id | available | product_name    | region_code | state_code | usoc  |
+----+-----------+-----------------+-------------+------------+-------+
|  1 | Y         | Silver Bullet   | Tampa       | TX         | j2345 |
|  2 | Y         | Platinum Bullet | Dallas      | TX         | j2345 |
|  3 | Y         | Steal Bullet    | Tampa       | TX         | j2345 |
+----+-----------+-----------------+-------------+------------+-------+
3 rows in set (0.00 sec)

mysql>

``

## _Looking deep into the pod and understanding the environment varibales generated_

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

```
kubectl cluster-info
Kubernetes master is running at https://34.66.62.137

GLBCDefaultBackend is running at https://34.66.62.137/api/v1/namespaces/kube-system/services/default-http-backend:http/proxy
Heapster is running at https://34.66.62.137/api/v1/namespaces/kube-system/services/heapster/proxy
KubeDNS is running at https://34.66.62.137/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://34.66.62.137/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
### INSTALLING KUBERNETES DASHBOARD

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml

```
For GCP Kubernetes Engine Clusters , it comes out of the box , enable it at the time of cluster creation

```
arun-mac:~ arunaja$ kubectl proxy
Starting to serve on 127.0.0.1:8001

```
```
Kubectl will make Dashboard available at http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.

```

**_login use access token option_

```
kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: REDACTED
    server: https://34.66.232.74
  name: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
contexts:
- context:
    cluster: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
    user: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
  name: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
current-context: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
kind: Config
preferences: {}
users:
- name: gke_kubernetes-01-222705_us-central1-a_standard-cluster-1
  user:
    auth-provider:
      config:
        access-token: ya29.Glz4Biw3XhQVIlMox5RdDR3dkfpqxF0h1NQnCGVRWAyIbH6VnrVdQ2AFm756CQ8ohK3gDrTiJfUWQHxcvTNaqpnKG9K502bEAdbsMCrKnfH8Yq78-ZGcbZc6-9MRYA
        cmd-args: config config-helper --format=json
        cmd-path: /Users/arunaja/google-cloud-sdk/bin/gcloud
        expiry: 2019-04-27T07:56:39Z
        expiry-key: '{.credential.token_expiry}'
        token-key: '{.credential.access_token}'
      name: gcp
      
```
     
   


   ![Kubernetes Dashboard View](https://github.com/ArunachalamJegadeesan/kubernetess-cookbook/blob/master/dashboard.png "Kubernetes Dashboard")
   
   
   
   
   
