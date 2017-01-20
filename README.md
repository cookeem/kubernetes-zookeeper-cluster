# How to deploy zookeeper cluster on kubernetes

---

### Prerequisites

- Install minikube or kubernetes

Follow this guide: [Install minikube on local machine](https://kubernetes.io/docs/getting-started-guides/minikube/)

Follow this guide: [Install kubernetes by kubeadm on cluster](https://kubernetes.io/docs/getting-started-guides/kubeadm/)

- Install docker-engine

Follow this guide: [Install Docker and run hello-world](https://docs.docker.com/engine/getstarted/step_one/)

---

### Clone the git repository

- Clone the git repository and change working directory to ```kubernetes-zookeeper-cluster/```

```sh
$ git clone https://github.com/cookeem/kubernetes-zookeeper-cluster.git

$ cd kubernetes-zookeeper-cluster/
```

---

### Deploy 3 nodes zookeeper cluster in kubernetes

- Use kubectl to deploy 3 nodes zookeeper cluster in kubernetes

```sh
$ kubectl create -f .
service "zk1" created
deployment "zk1" created
service "zk2" created
deployment "zk2" created
service "zk3" created
deployment "zk3" created
```


Check the pods and services:

```sh
$ kubectl get pods -l app=zk
NAME                           READY     STATUS    RESTARTS   AGE
zk1-4074261018-dbjnw           1/1       Running   0          1m
zk2-389827100-0mlrq            1/1       Running   0          1m
zk3-1002916382-6q8mt           1/1       Running   0          1m

$ kubectl get svc -l app=zk
NAME              CLUSTER-IP   EXTERNAL-IP   PORT(S)                                        AGE
zk1               10.0.0.230   <pending>     2181:32533/TCP,2888:30055/TCP,3888:32278/TCP   2m
zk2               10.0.0.45    <pending>     2181:30810/TCP,2888:32584/TCP,3888:31637/TCP   2m
zk3               10.0.0.18    <pending>     2181:32475/TCP,2888:32229/TCP,3888:31527/TCP   2m

```

Verify the cluster start correct or not

```sh
kubectl exec -ti zk1-4074261018-dbjnw -- /bin/bash
bash-4.3# zkCli.sh -server zk2:2181 ls /
Connecting to zk2:2181
2017-01-20 02:16:52,753 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.9-1757313, built on 08/23/2016 06:50 GMT
2017-01-20 02:16:52,757 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=zk1-4074261018-dbjnw
2017-01-20 02:16:52,759 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.8.0_111-internal
2017-01-20 02:16:52,767 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2017-01-20 02:16:52,768 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/usr/lib/jvm/java-1.8-openjdk/jre
2017-01-20 02:16:52,770 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/zookeeper-3.4.9/bin/../build/classes:/zookeeper-3.4.9/bin/../build/lib/*.jar:/zookeeper-3.4.9/bin/../lib/slf4j-log4j12-1.6.1.jar:/zookeeper-3.4.9/bin/../lib/slf4j-api-1.6.1.jar:/zookeeper-3.4.9/bin/../lib/netty-3.10.5.Final.jar:/zookeeper-3.4.9/bin/../lib/log4j-1.2.16.jar:/zookeeper-3.4.9/bin/../lib/jline-0.9.94.jar:/zookeeper-3.4.9/bin/../zookeeper-3.4.9.jar:/zookeeper-3.4.9/bin/../src/java/lib/*.jar:/conf:
2017-01-20 02:16:52,772 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/usr/lib/jvm/java-1.8-openjdk/jre/lib/amd64/server:/usr/lib/jvm/java-1.8-openjdk/jre/lib/amd64:/usr/lib/jvm/java-1.8-openjdk/jre/../lib/amd64:/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2017-01-20 02:16:52,772 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2017-01-20 02:16:52,774 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2017-01-20 02:16:52,775 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2017-01-20 02:16:52,776 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2017-01-20 02:16:52,777 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=4.4.14-boot2docker
2017-01-20 02:16:52,777 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2017-01-20 02:16:52,777 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2017-01-20 02:16:52,777 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/zookeeper-3.4.9
2017-01-20 02:16:52,782 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=zk2:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@69d0a921
2017-01-20 02:16:52,871 [myid:] - INFO  [main-SendThread(zk2.default.svc.cluster.local:2181):ClientCnxn$SendThread@1032] - Opening socket connection to server zk2.default.svc.cluster.local/10.0.0.45:2181. Will not attempt to authenticate using SASL (unknown error)
2017-01-20 02:16:52,953 [myid:] - INFO  [main-SendThread(zk2.default.svc.cluster.local:2181):ClientCnxn$SendThread@876] - Socket connection established to zk2.default.svc.cluster.local/10.0.0.45:2181, initiating session
2017-01-20 02:16:53,171 [myid:] - INFO  [main-SendThread(zk2.default.svc.cluster.local:2181):ClientCnxn$SendThread@1299] - Session establishment complete on server zk2.default.svc.cluster.local/10.0.0.45:2181, sessionid = 0x259b9a4bf980000, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zookeeper]
```

### Trouble shooting

- Zookeeper boot failure cause of k8s virtual ip 

```sh
        - name: ZOO_SERVERS
          # notices!!! k8s use virtual ip, ZOO_SERVERS variables value local pod must use 0.0.0.0 as the ip address, can not use hostname.
          # Otherwise ilocal pod zookeeper will boot failure and throw this exception:
          # ERROR [zk1/10.0.0.251:3888:QuorumCnxManager$Listener@547] - Exception while listening
          # java.net.BindException: Address not available (Bind failed)
          value: server.1=0.0.0.0:2888:3888 server.2=zk2:2888:3888 server.3=zk3:2888:3888
```
