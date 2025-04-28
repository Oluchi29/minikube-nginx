# minikube-nginx
a project to deploy nginx using minikube
minikube start
mkdir minikube-nginx
cd minikube-nginx
# Create pod.yaml and service.yaml inside the folder
kubectl create namespace dev
kubectl apply -f pod.yaml
kubectl apply -f service.yaml
kubectl get pods -n dev
kubectl get svc -n dev
minikube ip
# Visit http://<minikube-ip>:30080

# To delete all pods inside a namespace

kubectl delete pods --all -n dev

# . Delete the Namespace

kubectl delete namespace dev



## Errors Encountered
minikube start was not working 

 minikube start
W0428 19:19:26.955386    8296 main.go:291] Unable to resolve the current Docker CLI context "default": context "default": context not found: open C:\Users\USER\.docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba141d5f4133a33f0688f\meta.json: The system cannot find the path specified.
😄  minikube v1.34.0 on Microsoft Windows 10 Pro 10.0.21996.1 Build 21996.1
✨  Using the  driver based on existing profile

❌  Exiting due to DRV_UNSUPPORTED_OS: The driver '' is not supported on windows/amd64

## Solution
minikube start --driver=docker

Corrected the error and minikube started working

## Second error encountered

minikube ip


was showing connection error on the browser

# troubleshooting

minikube status (to check if pods nd everything is running)
secondly, service details
kubectl get svc -n dev

NAME          TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
web-service   NodePort   10.108.54.218   <none>        80:30080/TCP     5m

my port column is showing so my problem was not from here

thirdly,  Try port-forwarding as an alternative

If using minikube ip and nodePort doesn't work (rare but can happen on some Windows setups), you can port-forward directly:

kubectl port-forward pod/web-pod 8080:80 -n dev

 kubectl port-forward pod/web-pod 8080:80 -n dev
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080
Handling connection for 8080


ran localhost:8080 (This forces local traffic straight into the Pod.)
on the browser voilla it displayed 
WELCOME TO NGINX
