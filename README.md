# CKAD exam practice questions and commands 

This repo is consolidated list of ckad excersises and commands 

#### Setup your environment to save sometime 
```
alias k=kubectl 
k config set-context $(k config current-context) --namespace=<namespace>
```
#### If you want to setup your favorite editor 
```
export KUBE_EDITOR=nano

```
#### Be familiar with below kubectl commands to create a pod with env variable, running on port 80, requests and limits, labels  
```
k run mypod --image=nginx --restart=Never --port=80 --env="name=value" --requests=cpu=50m,memory=50Mi --limits=cpu=100m,memory=100Mi --replicas=1 --labels=exam=ckad --dry-run -o yaml -- /bin/bash -c 
'echo Hello; sleep 3600' 

```

#### To create a deployment with similar attributes 
```
k run mydeploy --image=nginx  --port=80 --env="name=value" --requests=cpu=50m,memory=50Mi --limits=cpu=100m,memory=100Mi --replicas=1 --labels=exam=ckad --dry-run -o yaml -- /bin/bash -c  'echo Hello; sleep 3600'  

```
#### To create a cronjob 
```
k run cronjob --image=nginx --labels=exam=ckad --restart=OnFailure --schedule="*/1 * * * * " -- /bin/bash -c 'echo Hello CronJob'
```
### To create a job 
```
k run job --image=nginx --labels=exam=ckad --restart=OnFailure -- /bin/bash -c 'echo Hello job'
