

kubectl-user auth can-i use podsecuritypolicy/example

kubectl create namespace psp-example
kns psp-example    
kubectl create serviceaccount -n psp-example fake-user
kubectl create rolebinding -n psp-example fake-editor --clusterrole=edit --serviceaccount=psp-example:fake-user

k apply -n psp-example -f app-psp.yaml

alias kubectl-admin='kubectl -n psp-example'
alias kubectl-user='kubectl --as=system:serviceaccount:psp-example:fake-user -n psp-example'

k apply -f app-psp.yaml

kubectl-user create -f- <<EOF
apiVersion: v1
kind: Pod
metadata:
  name:      pause
spec:
  containers:
    - name:  pause
      image: k8s.gcr.io/pause
EOF
Error from server (Forbidden): error when creating "STDIN": pods "pause" is forbidden: unable to validate against any pod security policy: []


kubectl-user auth can-i use podsecuritypolicy/example


kubectl-admin create role psp:unprivileged \
  --verb=use \
  --resource=podsecuritypolicy \
  --resource-name=app-psp

            
kubectl-admin create rolebinding fake-user:psp:unprivileged \
  --role=psp:unprivileged \
  --serviceaccount=psp-example:fake-user

                    
                    


kubectl-user create -f- <<EOF
apiVersion: v1
kind: Pod
metadata:
  name:      pause
spec:
  containers:
    - name:  pause
      image: k8s.gcr.io/pause
EOF
pod "pause" created


kubectl-user create -f- <<EOF
apiVersion: v1
kind: Pod
metadata:
  name:      privileged
spec:
  containers:
    - name:  pause
      image: k8s.gcr.io/pause
      securityContext:
        privileged: true
EOF


kubectl-user run pause --image=k8s.gcr.io/pause

the pod won't created because default serviceaccount is not authorized

kubectl-admin create rolebinding default:psp:unprivileged \
    --role=psp:unprivileged \
    --serviceaccount=psp-example:default
    
    