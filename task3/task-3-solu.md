# DevOps Assignment 4: Task 3

**Trainee Name: Yasser Ahmed**  
**Group: ALX2_SWD1_G1**  

## Table of Contents

1. [How many ConfigMaps exist in the environment?](#1--how-many-configmaps-exist-in-the-environment)
2. [Create a new ConfigMap Use the spec given below.](#2--create-a-new-configmap-use-the-spec-given-below)
3. [Create a webapp-color POD with nginx image and use the created ConfigMap](#3--create-a-webapp-color-pod-with-nginx-image-and-use-the-created-configmap)
4. [How many Secrets exist on the system?](#4--how-many-secrets-exist-on-the-system)
5. [How many secrets are defined in the default-token secret?](#5--how-many-secrets-are-defined-in-the-default-token-secret)
6. [Create a POD called db-pod with the image mysql:5.7 then check the POD status](#6--create-a-pod-called-db-pod-with-the-image-mysql57-then-check-the-pod-status)
7. [Why the db-pod status not ready](#7--why-the-db-pod-status-not-ready)
8. [Create a new secret named db-secret with the data given below](#8--create-a-new-secret-named-db-secret-with-the-data-given-below)
9. [Configure db-pod to load environment variables from the newly created secret](#9--configure-db-pod-to-load-environment-variables-from-the-newly-created-secret)
10. [Create a multi-container pod with 2 containers](#10--create-a-multi-container-pod-with-2-containers)
11. [Create a pod red with redis image and use an initContainer that uses the busybox image and sleeps for 20 seconds](#11--create-a-pod-red-with-redis-image-and-use-an-initcontainer-that-uses-the-busybox-image-and-sleeps-for-20-seconds)
12. [Create a pod named print-envars-greeting](#12--create-a-pod-named-print-envars-greeting)
13. [Where is the default kubeconfig file located in the current environment?](#13--where-is-the-default-kubeconfig-file-located-in-the-current-environment)
14. [How many clusters are defined in the default kubeconfig file?](#14--how-many-clusters-are-defined-in-the-default-kubeconfig-file)
15. [What is the user configured in the current context?](#15--what-is-the-user-configured-in-the-current-context)
16. [Create a Persistent Volume with the given specification](#16--create-a-persistent-volume-with-the-given-specification)
17. [Create a Persistent Volume Claim with the given specification](#17--create-a-persistent-volume-claim-with-the-given-specification)
18. [Create a webapp pod to use the persistent volume claim as its storage](#18--create-a-webapp-pod-to-use-the-persistent-volume-claim-as-its-storage)

### 1- How many ConfigMaps exist in the environment?

```bash
yasser@kane:~/k8s$ k get cm
NAME               DATA   AGE
kube-root-ca.crt   1      2d6h
```

---

### 2- Create a new ConfigMap Use the spec given below

- ConfigName Name: webapp-config-map
- Data: APP_COLOR=darkblue

```bash
yasser@kane:~/k8s$ k create cm webapp-config-map --from-literal=APP_COLOR=darkblue
configmap/webapp-config-map created

yasser@kane:~/k8s$ k get cm webapp-config-map
NAME                DATA   AGE
webapp-config-map   1      2m49s

yasser@kane:~/k8s$ k get cm webapp-config-map -o yaml
apiVersion: v1
data:
  APP_COLOR: darkblue
kind: ConfigMap
metadata:
  creationTimestamp: "2025-02-11T16:57:33Z"
  name: webapp-config-map
  namespace: default
  resourceVersion: "19013"
  uid: 945bb882-3cfe-4f4f-ab4d-992450cd3851
```

---

### 3- Create a webapp-color POD with nginx image and use the created ConfigMap

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp-color
spec:
  containers:
  - name: webapp-color-c
    image: nginx
    env:
    - name: APP_COLOR
      valueFrom:
        configMapKeyRef:
          name: webapp-config-map
          key: APP_COLOR
    command:
    - "/bin/sh"
    - "-c"
    args:
    - "echo 'The color of the app is: ' $APP_COLOR"
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/webapp-color-pod.yaml 
pod/webapp-color created

yasser@kane:~/k8s$ k logs webapp-color
The color of the app is:  darkblue
```

---

### 4- How many `Secrets` exist on the system?

None

```bash
yasser@kane:~/k8s$ k get secrets
No resources found in default namespace.

yasser@kane:~/k8s$ k get secrets --all-namespaces
No resources found
```

---

### 5- How many secrets are defined in the default-token secret?

```bash
yasser@kane:~/k8s$ k get secret default-token
Error from server (NotFound): secrets "default-token" not found
```

---

### 6- create a POD called db-pod with the image mysql:5.7 then check the POD status

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
spec:
  containers:
  - name: db-pod-c
    image: mysql:5.7
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: password
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/db-pod.yaml 
pod/db-pod created

yasser@kane:~/k8s$ k get pod db-pod
NAME     READY   STATUS    RESTARTS   AGE
db-pod   1/1     Running   0          2m30s
```

---

### 7- why the db-pod status not ready

```bash
yasser@kane:~/k8s$ k get pod db-pod
NAME     READY   STATUS    RESTARTS   AGE
db-pod   1/1     Running   0          13m
```

---

### 8- Create a new secret named db-secret with the data given below

- Secret Name: db-secret
- Secret 1: MYSQL_DATABASE=sql01
- Secret 2: MYSQL_USER=user1
- Secret 3: MYSQL_PASSWORD=password
- Secret 4: MYSQL_ROOT_PASSWORD=password123

```bash
yasser@kane:~/k8s$ k create secret generic db-secret \
  --from-literal=MYSQL_DATABASE=sql01 \
  --from-literal=MYSQL_USER=user1 \
  --from-literal=MYSQL_PASSWORD=password \
  --from-literal=MYSQL_ROOT_PASSWORD=password123
secret/db-secret created

yasser@kane:~/k8s$ k get secret db-secret -o yaml
apiVersion: v1
data:
  MYSQL_DATABASE: c3FsMDE=
  MYSQL_PASSWORD: cGFzc3dvcmQ=
  MYSQL_ROOT_PASSWORD: cGFzc3dvcmQxMjM=
  MYSQL_USER: dXNlcjE=
kind: Secret
metadata:
  creationTimestamp: "2025-03-03T15:41:02Z"
  name: db-secret
  namespace: default
  resourceVersion: "24355"
  uid: 5c6ed4ca-4cfe-4ac2-adcc-2ec8f5cb0c18
type: Opaque


yasser@kane:~/k8s$ echo "cGFzc3dvcmQxMjM=" | base64 --decode
password123
```

---

### 9- Configure db-pod to load environment variables from the newly created secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
spec:
  containers:
  - name: db-pod-c
    image: mysql:5.7
    env:
    - name: MYSQL_DATABASE
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: MYSQL_DATABASE
    - name: MYSQL_USER
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: MYSQL_USER
    - name: MYSQL_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: MYSQL_PASSWORD
    - name: MYSQL_ROOT_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: MYSQL_ROOT_PASSWORD
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/db-pod.yaml 
pod/db-pod created

yasser@kane:~/k8s$ k get pod db-pod
NAME     READY   STATUS    RESTARTS   AGE
db-pod   1/1     Running   0          76s

yasser@kane:~/k8s$ k exec db-pod -- env | grep MYSQL_
MYSQL_DATABASE=sql01
MYSQL_USER=user1
MYSQL_PASSWORD=password
MYSQL_ROOT_PASSWORD=password123
MYSQL_MAJOR=5.7
MYSQL_VERSION=5.7.44-1.el7
MYSQL_SHELL_VERSION=8.0.35-1.el7
```

---

### 10- Create a multi-container pod with 2 containers

- Name: yellow
  - Container 1
    - Name: lemon
    - Image: `busybox`
  - Container 2
    - Name: gold
    - Image: `redis`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: yellow
spec:
  containers:
  - name: lemon
    image: busybox
  - name: gold
    image: redis
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/yellow.yaml 
pod/yellow created

yasser@kane:~/k8s$ k get pod yellow
NAME     READY   STATUS             RESTARTS      AGE
yellow   1/2     CrashLoopBackOff   3 (46s ago)   2m51s
```

---

### 11- Create a pod `red` with `redis` image and use an `initContainer` that uses the `busybox` image and sleeps for 20 seconds

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: red
spec:
  containers:
  - name: red
    image: redis
  initContainers:   # initContainers are executed before the main container starts
  - name: init
    image: busybox
    command: ['sh', '-c', 'sleep 20']
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/red.yaml 
pod/red created

yasser@kane:~/k8s$ k get pod red
NAME   READY   STATUS     RESTARTS   AGE
red    0/1     Init:0/1   0          9s

yasser@kane:~/k8s$ k get pod red
NAME   READY   STATUS    RESTARTS   AGE
red    1/1     Running   0          27s
```

---

### 12- Create a pod named print-envars-greeting

1. Configure spec as, the container name should be
print-env-container and use bash image.
2. Create three environment variables:
a. GREETING and its value should be “Welcome to”
b. COMPANY and its value should be “DevOps”
c. GROUP and its value should be “Industries”
3. Use command to echo ["$(GREETING) $(COMPANY) $(GROUP)"]
message.
4. You can check the output using <kubctl logs -f [ pod-name ]>
command.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
  - name: print-env-container
    image: bash
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "DevOps"
    - name: GROUP
      value: "Industries"
    command: ["/bin/sh", "-c"]
    args:
    - echo $GREETING $COMPANY $GROUP
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/print-envars-greeting.yaml 
pod/print-envars-greeting created

yasser@kane:~/k8s$ k get pod print-envars-greeting
NAME                    READY   STATUS              RESTARTS   AGE
print-envars-greeting   0/1     ContainerCreating   0          2s

yasser@kane:~/k8s$ k get pod print-envars-greeting
NAME                    READY   STATUS      RESTARTS   AGE
print-envars-greeting   0/1     Completed   0          6s

yasser@kane:~/k8s$ k get pod print-envars-greeting
NAME                    READY   STATUS             RESTARTS     AGE
print-envars-greeting   0/1     CrashLoopBackOff   1 (3s ago)   8s

yasser@kane:~/k8s$ k logs print-envars-greeting
Welcome to DevOps Industries

yasser@kane:~/k8s$ k logs -f print-envars-greeting
Welcome to DevOps Industries
```

---

### 13- Where is the default kubeconfig file located in the current environment?

at ~/.kube/config (for minikube also)

```bash
yasser@kane:~/k8s$ cat ~/.kube/config 
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/yasser/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/yasser/.minikube/profiles/minikube/client.crt
    client-key: /home/yasser/.minikube/profiles/minikube/client.key

yasser@kane:~/k8s$ k config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/yasser/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/yasser/.minikube/profiles/minikube/client.crt
    client-key: /home/yasser/.minikube/profiles/minikube/client.key
```

---

### 14- How many clusters are defined in the default kubeconfig file?

only one cluster called minikube

```yaml
yasser@kane:~/k8s$ k config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/yasser/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Mon, 03 Mar 2025 19:25:55 EET
        provider: minikube.sigs.k8s.io
        version: v1.34.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/yasser/.minikube/profiles/minikube/client.crt
    client-key: /home/yasser/.minikube/profiles/minikube/client.key

yasser@kane:~/k8s$ k config view -o jsonpath='{.clusters[*].name}' 
minikube

yasser@kane:~/k8s$ k config view -o jsonpath='{.clusters[*].name}' | wc -w
1

```

---

### 15- What is the user configured in the current context?

minikube

```bash
yasser@kane:~/k8s$ k config current-context
minikube
```

---

### 16- Create a Persistent Volume with the given specification

- Volume Name: pv-log
- Storage: 100Mi
- Access Modes: ReadWriteMany
- Host Path: /pv/log

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/log
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/pv-log.yaml 
persistentvolume/pv-log created

yasser@kane:~/k8s$ k get pv pv-log
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
pv-log   100Mi      RWX            Retain           Available                          <unset>                          18s
```

---

### 17- Create a Persistent Volume Claim with the given specification

- Volume Name: claim-log-1
- Storage Request: 50Mi
- Access Modes: ReadWriteMany

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/claim-log-1.yaml 
persistentvolumeclaim/claim-log-1 created

yasser@kane:~/k8s$ k get pvc claim-log-1
NAME          STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
claim-log-1   Bound    pvc-49e30b22-8041-4e0c-9115-30ee6933e956   50Mi       RWX            standard       <unset>                 20s
```

---

### 18- Create a webapp pod to use the persistent volume claim as its storage

- Name: webapp
- Image Name: nginx
- Volume: PersistentVolumeClaim=claim-log-1
- Volume Mount: /var/log/nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: webapp-c
    image: nginx
    volumeMounts:
    - mountPath: /var/log/nginx
      name: claim-log-1
  volumes:
  - name: claim-log-1
    persistentVolumeClaim:
      claimName: claim-log-1
```

```bash
yasser@kane:~/k8s$ k create -f assignment-4/task-3/webapp.yaml 
pod/webapp created

yasser@kane:~/k8s$ k get pod webapp
NAME     READY   STATUS    RESTARTS   AGE
webapp   1/1     Running   0          14s

yasser@kane:~/k8s$ k exec -it webapp -- ls /var/log/nginx
access.log  error.log
```
