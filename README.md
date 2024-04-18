# argo-workflow-system-tests
System test repo for system tests defined using  argo workflows.


# Prerequisities
1.) Install Argo Workflow
2.) Install Argo CLI


## Install argo workflow
Create argo namespace
`kubectl create namespace arg`

Install the latest version of argo workflow
`kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.5.5/install.yaml`


## Install argo cli
Download the binary
`curl -sLO https://github.com/argoproj/argo-workflows/releases/download/v3.5.5/argo-linux-amd64.gz`

Unzip
`gunzip argo-linux-amd64.gz`

Make binary executable
`chmod +x argo-linux-amd64`

Move binary to path
`mv ./argo-linux-amd64 /usr/local/bin/argo`

Test installation
`argo version`



# Executing a argo workflow using argo cli
Execute workflow defined in a yaml file
`argo submit -n argo --watch <yaml file> --serviceaccount argo`


# Executing argo workflow using UI
`kubectl -n argo port-forward deployment/argo-server 2746:2746`

