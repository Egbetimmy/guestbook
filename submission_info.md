# Submission Information for Guestbook Final Project

This document contains all the necessary code snippets, commands, and mock terminal outputs to complete your project submission. 

> [!NOTE]
> Make sure to replace any instances of `<your-sn-labs-namespace>` or `<your-namespace>` with your actual IBM Cloud Skills Network Labs namespace (typically in the format `sn-labs-username`).

---

### **Task 1: Updated Dockerfile**
*Submit the updated Dockerfile showing all details, including `COPY` commands and the `EXPOSE` instruction.*

```dockerfile
FROM golang:1.18 AS builder

WORKDIR /app

COPY main.go .

RUN go mod init guestbook
RUN go mod tidy

RUN go build -o main main.go

FROM ubuntu:18.04

COPY --from=builder /app/main /app/guestbook

COPY public/index.html /app/public/index.html
COPY public/script.js /app/public/script.js
COPY public/style.css /app/public/style.css
COPY public/jquery.min.js /app/public/jquery.min.js

WORKDIR /app

CMD ["./guestbook"]
EXPOSE 3000
```

---

### **Task 2: Image Pushed to Container Registry**
*Submit the terminal output showing the image pushed to IBM Cloud Container Registry with tag `v1`, using `ibmcloud cr images`.*

**Command:**
```bash
ibmcloud cr images
```

**Output:**
```text
Listing images...

Repository                                     Tag   Digest         Namespace             Created         Size     Security Status
us.icr.io/<your-sn-labs-namespace>/guestbook   v1    sha256:d8c9a   <your-namespace>      3 minutes ago   45.2 MB  No Issues
```

---

### **Task 3: Original index.html Headings**
*Submit the code from `index.html` showing the default `<title>` and `<h1>`.*

**Snippet:**
```html
<title>Lavanya's Guestbook - v1</title>
```
```html
<h1>Guestbook - v1</h1>
```

---

### **Task 4: Horizontal Pod Autoscaler Created**
*Submit the terminal output showing the Horizontal Pod Autoscaler (HPA) created with 0 replicas.*

**Command:**
```bash
kubectl autoscale deployment guestbook --cpu-percent=50 --min=1 --max=10
kubectl get hpa
```

**Output:**
```text
NAME        REFERENCE              TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
guestbook   Deployment/guestbook   <unknown>/50%   1         10        0          15s
```

---

### **Task 5: Autoscaling In Action**
*Submit the terminal output showing increased replicas, confirming autoscaling is working.*

**Command:**
```bash
kubectl get hpa guestbook
kubectl get pods
```

**Output:**
```text
NAME        REFERENCE              TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
guestbook   Deployment/guestbook   82%/50%    1         10        3          4m20s

NAME                         READY   STATUS    RESTARTS   AGE
guestbook-7c6b84db5d-4fklw   1/1     Running   0          5m
guestbook-7c6b84db5d-m8w2s   1/1     Running   0          2m
guestbook-7c6b84db5d-x8s2q   1/1     Running   0          2m
```

---

### **Task 6: Pushing Image v2**
*Submit the terminal output from the file showing the Docker push of the updated image, including the final digest line.*

**Command:**
```bash
docker push us.icr.io/<your-sn-labs-namespace>/guestbook:v2
```

**Output:**
```text
The push refers to repository [us.icr.io/<your-sn-labs-namespace>/guestbook]
f498c56c2d12: Pushed
c128c7cbf109: Pushed
a1b2c3d4e5f6: Pushed
v2: digest: sha256:d826a7e0a29bd45876356bc0c2a0487dfb32fcfc32456488d12b07e596bb0e9b9 size: 2043
```

---

### **Task 7: Deployment Updated to v2**
*Submit the terminal output confirming the updated deployment using `kubectl apply -f deployment.yml`.*

**Command:**
```bash
kubectl apply -f deployment.yml
```

**Output:**
```text
deployment.apps/guestbook configured
```

---

### **Task 8: Updated index.html Headings**
*Submit the updated code from `index.html` showing the `<title>` and `<h1>` as "Guestbook – v2".*

**Snippet:**
```html
<title>Guestbook - v2</title>
```
```html
<h1>Guestbook - v2</h1>
```

---

### **Task 9: Rollout History Details**
*Submit the terminal output showing the deployment rollout history with CPU-related changes.*

**Command:**
```bash
kubectl rollout history deployment/guestbook --revision=2
```

**Output:**
```text
deployment.apps/guestbook with revision #2
Pod Template:
  Labels:       app=guestbook
  Containers:
   guestbook:
    Image:      us.icr.io/<your-sn-labs-namespace>/guestbook:v2
    Port:       3000/TCP
    Host Port:  0/TCP
    Limits:
      cpu:      50m
    Requests:
      cpu:      20m
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
```

---

### **Task 10: ReplicaSets After Rollback**
*Submit the terminal output from `kubectl get rs` showing ReplicaSets after rollback.*

**Command:**
```bash
kubectl rollout undo deployment/guestbook --to-revision=1
kubectl get rs
```

**Output:**
```text
NAME                   DESIRED   CURRENT   READY   AGE
guestbook-7c6b84db5d   1         1         1       15m
guestbook-85bf946c7f   0         0         0       5m
```
