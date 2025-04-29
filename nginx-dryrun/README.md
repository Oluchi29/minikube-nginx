You have just being newly employed as a cloud engr level 1,You have the following task

1.Using a single node  minikube cluster or any kubernetes cluster of your choice

Create a pod named web-pod
with image: ngnix
Namesapace: dev
.Expose the pod on port:80
Make sure the pod is not running meaning you a do a dry run and output the yaml file on pod.yaml.

2. Now create (apply the file using this pod.yaml file.
3. Check the status of the pod, troubleshoot and correct the issues with the pod and reconfigure appropriately stating your troubleshooting processes to resolving the issues
4. If and when the issues are resolved ,you must exec into the pod to show what's inside the pod but make sure the pod is running

Hint:
alias k=kubectl
export do="- - dry-run=client -o yaml"

## Steps to deploy this project
With minikube already installed on your local machine run

minikube start
alias k=kubectl
export do="--dry-run=client -o yaml"
k create namespace dev
k run web-pod --image=ngnix --port=80 -n dev $do > pod.yaml (This will not create the pod yet — it just generates the YAML file)
k apply -f pod.yaml
k get pods -n dev
k describe pod web-pod -n dev
# Fix pod.yaml (change ngnix -> nginx)
k delete -f pod.yaml
k apply -f pod.yaml
k get pods -n dev
k exec -it web-pod -n dev -- sh

Once you,ve successfully exec into the pod do "ls" to know what is inside the pod
Next, you exit



## Errors encountered
minikube start displayed

 minikube start
W0429 08:37:55.269772   18252 main.go:291] Unable to resolve the current Docker CLI context "default": context "default": context not found: open C:\Users\USER\.docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba141d5f4133a33f0688f\meta.json: The system cannot find the path specified.
😄  minikube v1.34.0 on Microsoft Windows 10 Pro 10.0.21996.1 Build 21996.1
✨  Using the  driver based on existing profile

❌  Exiting due to DRV_UNSUPPORTED_OS: The driver '' is not supported on windows/amd64


The below command started minikube
minikube start --driver=docker

## Second error

 k get pods -n dev
NAME      READY   STATUS         RESTARTS   AGE
web-pod   0/1     ErrImagePull   0          98s

ran the below command
k describe pod web-pod -n dev

 k describe pod web-pod -n dev
Name:             web-pod
Namespace:        dev
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Tue, 29 Apr 2025 09:05:10 +0100
Labels:           run=web-pod
Annotations:      <none>
Status:           Pending
IP:               10.244.0.8
IPs:
  IP:  10.244.0.8
Containers:
  web-pod:
    Container ID:
    Image:          ngnix
    Image ID:
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-2g9hg (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       False
  ContainersReady             False
  PodScheduled                True
Volumes:
  kube-api-access-2g9hg:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Scheduled  2m51s                default-scheduler  Successfully assigned dev/web-pod to minikube
  Normal   Pulling    49s (x4 over 2m42s)  kubelet            Pulling image "ngnix"
  Warning  Failed     39s (x4 over 2m27s)  kubelet            Failed to pull image "ngnix": Error response from daemon: pull access denied for ngnix, repository does not exist or may require 'docker login': denied: requested access to the resource is denied 
  Warning  Failed     39s (x4 over 2m27s)  kubelet            Error: ErrImagePull
  Warning  Failed     24s (x6 over 2m26s)  kubelet            Error: ImagePullBackOff
  Normal   BackOff    11s (x7 over 2m26s)  kubelet            Back-off pulling image "ngnix"

  ## SOLUTION
  change the image name to nginx
## Note once u initialize the change in your pod.yaml 
delete ehe existing pod before creating another one
secondly when you,ve created the pod if your 
k get pods is showing containercreating dont panic give it a little time mine took 8mins before the pod started running.