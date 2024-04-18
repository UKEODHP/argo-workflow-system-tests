# argo-workflow-system-tests
System test repo for system tests defined using  argo workflows.


# Prerequisities
1.) Install Argo Workflow

2.) Install Argo CLI


## Install argo workflow
Create argo namespace

`kubectl create namespace argo`

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

Navigate your browser to Workflow UI using 

`https://localhost::2746/`

Due to the self-signed certificate, you will receive a TLS error which you will need to manually approve.

Click `+ Submit New Workflow` and then `Edit using full workflow options`

You can find an example workflow already in the text field. Press `+ Create` to start the workflow.
