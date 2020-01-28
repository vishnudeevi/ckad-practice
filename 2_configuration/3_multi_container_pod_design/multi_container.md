10% - Multi-container pods 
* understand multi-container pod design patterns (sidecar, adapter and ammbasdor)
### Bookmark below links 
* [pod design with samples](https://matthewpalmer.net/kubernetes-app-developer/articles/multi-container-pod-design-patterns.html)
* [multi container from tasks](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/#creating-a-pod-that-runs-two-containers)
#### Create a pod with two busbox containers that would log 'Hello from container-1' and 'Hello from container-2' and sleep 3600
```bash
k run multipod --image=busybox --restart=Never --dry-run -o yaml -- /bin/sh -c 'while true; do date||'\multipod-container-1'\ >> /var/log/app.txt; sleep 5;done' > pod.yaml 
```
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: multipod
  name: multipod
spec:
  volumes:
  - name: myvol
    emptyDir: {}
  containers:
  - args:
    - /bin/sh
    - -c
    -  while true; do date >> /var/log/app1.txt; sleep 5;done
    image: busybox
    name: multipod1
    resources: {}
    volumeMounts:
    - name: myvol
      mountPath: /var/log
  - args:
    - /bin/sh
    - -c
    -  while true; do date >> /var/log/app2.txt;sleep 1;done;
    image: busybox
    name: multipod2
    resources: {}
    volumeMounts:
    - name: myvol
      mountPath: /var/log
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
```bash 
k apply -f pod.yaml
k exec -it multipod -c multipod1 -- /bish/sh 
tail -f -n 20 /var/log/app1.txt  # Log app1 spit by multipod1 container every 5 seconds 
tail -f -n 20 /var/log/app2.txt  # Logs app2 spit by multipod2 container every 1 seconds and are loaded into same volume 
```