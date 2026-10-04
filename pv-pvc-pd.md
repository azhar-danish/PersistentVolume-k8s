
1. Creating a Persistent Volume (PV) in Kubernetes allows you to define a storage resource that can be used by your pods. Below is an example of a YAML configuration for a Persistent Volume:

apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-pv-volume
  labels:
    type: local
spec: 
  storageClassName: "" # Matches your PVC standard setup
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data

2. Creating a Persistent Volume Claim (PVC) allows your pods to request storage resources defined by the PV. Below is an example of a YAML configuration for a Persistent Volume Claim: 

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-pv-claim
spec:
  storageClassName: ""
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi    

3. Creating a Pod that uses the Persistent Volume Claim allows your application to utilize the storage defined by the PV. Below is an example of a YAML configuration for a Pod that mounts the PVC:


apiVersion: v1
kind: Pod
metadata: 
  name: nginx-pod
  labels:
    env: demo
    type: backend
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
      - containerPort: 80
      volumeMounts:
      - mountPath: "/usr/share/nginx/html"
        name: test-pv-storage
  volumes:
    - name: test-pv-storage
      persistentVolumeClaim: 
        claimName: test-pv-claim
  nodeName: cka-bestseller-cluster2-control-plane

4. kubectl get pv
5. kubectl get pvc
6. kubectl get pods
7. kubectl exec -it nginx-pod -- /bin/bash
    cd /usr/share/nginx/html
    echo "Hello from Persistent Volume" > index.html
    echo "hello worlod" > hello.txt
8. kubectl delete pod nginx-pod
9. kubectl get pods
10. kubectl apply -f nginx-pod.yaml
11. kubectl exec -it nginx-pod -- /bin/bash
    cd /usr/share/nginx/html
    cat index.html
    cat hello.txt

