# DevOps Assignment 4: Task 3

**Trainee Name: Yasser Ahmed**  
**Group: ALX2_SWD1_G1**  

## Table of Contents

1. [](#)

### 1- How many ConfigMaps exist in the environment?

```bash
yasser@kane:~/k8s$ k get cm
NAME               DATA   AGE
kube-root-ca.crt   1      2d6h
```

### 2- Create a new ConfigMap Use the spec given below. [ConfigName Name: webapp-config-map, Data: APP_COLOR=darkblue]

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

### 8- Create a new secret named db-secret with the data given below.
- Secret Name: db-secret
- Secret 1: MYSQL_DATABASE=sql01
- Secret 2: MYSQL_USER=user1
- Secret 3: MYSQL_PASSWORD=password
- Secret 4: MYSQL_ROOT_PASSWORD=password123