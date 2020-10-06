## Install tools

Install minikube https://kubernetes.io/docs/tasks/tools/install-minikube/

    brew install minikube

Install helm

    brew install helm

Install ngrok for easy local port forwarding

    brew cask install ngrok

## Init tools

Start minikube

    minikube start

Create drone namespace

    kubectl create ns drone
    
Add drone helm chart repo

    helm repo add drone https://charts.drone.io

## Install drone server

Forward local port with ngrok

    ngrok http 127.0.0.1:8080

Update Bitbucket (GitHub, ...) OAuth url, webhook url, as well as drone server host with temporary ngrok url

Install drone server with helm

    helm install --namespace drone drone drone/drone -f drone-server-bitbucket.yaml
    
If you get the error `Error: This command needs 1 argument: chart name` try adding `--name drone` 

Optional: Upgrade existing if name is already in use

    helm upgrade --install drone drone/drone --namespace drone --values drone-server-bitbucket.yaml

Optional: Change existing config

    helm upgrade drone drone/drone --namespace drone --values drone-server-bitbucket.yaml

Set current pod name as variable DRONE_POD_NAME

    export DRONE_POD_NAME=$(kubectl get pods --namespace drone -l "app.kubernetes.io/name=drone,app.kubernetes.io/instance=drone" -o jsonpath="{.items[0].metadata.name}")

Port forwarding k8s -> outside

    kubectl --namespace drone port-forward $DRONE_POD_NAME 8080:80

WARNING: This allows any user with read access to secrets or the ability to create a pod to access super-user credentials. https://stackoverflow.com/a/49174177/3012347

    kubectl create clusterrolebinding serviceaccounts-cluster-admin \
      --clusterrole=cluster-admin \
      --group=system:serviceaccounts

create service entrypoint manually

    kubectl apply -f deployment.yaml


## Install drone runner

Install drone runner with helm

    helm install --namespace drone drone-runner-kube drone/drone-runner-kube -f drone-runner-kube-values.yaml
    
Optional: Upgrade existing if changed 

    helm upgrade --install --namespace drone drone-runner-kube drone/drone-runner-kube -f drone-runner-kube-values.yaml

### Setup repository secrets in drone

List drone secrets

    kubectl -n drone get secrets

Copy default service account token and replace $DEFAULT_SERVICE_ACCOUNT_TOKEN

    kubectl -n drone get secret/$DEFAULT_SERVICE_ACCOUNT_TOKEN -o yaml | egrep 'ca.crt:|token:'

Create new repository secret in drone with key `KUBERNETES_CERT` and value of `ca.crt`

Create new repository secret in drone with key `KUBERNETES_TOKEN` and base 64 encoded value of `token`
    
    echo $TOKEN | base64 -d && echo''

