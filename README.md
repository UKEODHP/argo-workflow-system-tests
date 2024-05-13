# argo-workflow-system-tests
System test repo for system tests defined using  argo workflows.


# Prerequisities
1.) Install Argo Workflow

## Install argo workflow

The argo workflow is deployed by argocd, below are optional commands to install it manually.

Create argo namespace

`kubectl create namespace argo`

Install the latest version of argo workflow

`kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.5.6/install.yaml`


## Install argo cli
Argo CLI is installed when the argo-workflow is installed above. No need to deploy it separately. Below commands are optional, showing how to deploy argo cli manually.

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

## passing parameter into argo workflow from argo submit command

`argo submit -n argo --watch <yaml file> -p <parametername>=<parametervalue> --serviceaccount argo`

e.g.
`argo submit -n argo --watch homepage-test.yaml -p url="https://dev.eodatahub.org.uk" --serviceaccount argo`

## reading parameters from a file

`argo submit -n argo --watch <yaml file> --parameter-file <parameterfilename> --serviceaccount argo`

e.g.
`argo submit -n argo --watch <yaml file> --parameter-file urlfile.yaml --serviceaccount argo`

where, the urlfile.yaml is
`url: https://dev.eodatahub.org.uk`



# Executing argo workflow using UI

`kubectl -n argo port-forward deployment/argo-server 2746:2746`

Navigate your browser to Workflow UI using 

`https://localhost::2746/`

Due to the self-signed certificate, you will receive a TLS error which you will need to manually approve.

Click `+ Submit New Workflow` and then `Edit using full workflow options`

You can find an example workflow already in the text field. Press `+ Create` to start the workflow.


# WorkflowTemplates

## Creating workflowtemplates

`argo template create url-test-workflowtemplate.yaml -n argo`

Create workflow which includes reference to workflowtemplate as usual 

`argo submit -n argo --watch -p url="https://dev.eodatahub.org.uk/" url-up-and-running-workflow.yaml --serviceaccount=argo`


# Cron Workflows
Cron workflows are used to schedule executing of a workflow in a specific time


## Creating Cron workflows

`argo cron create <manifestfile>`

e.g.
`argo cron create url-up-and-running-cron.yaml -p url="https://dev.eodatahub.org.uk"`
`argo crom create homepage-test-fromfile-cron.yaml`

## List created cron workflows

`argo cron list -n argo`

## get details of cron workflow

`argo cron get <cron workflow name> -n argo`

e.g.

`argo cron get url-up-and-running-workflow-5qpqk -n argo`

## delete argo cronworkflow
`argo cron delete <cron workflow name define in manifest> -n argo`

e.g.
`argo cron delete url-test-cronworkflow -n argo`


## delete argo workflows
list workflows created
`argo list -n argo`

then delete the one selected
`argo delete <name of the workflow> -n argo`


## creating configmap with json file content
`kubectl create configmap urlnames-configmap --from-file=urlnames.json -n argo`

the `urlnames-configmap` can then be used as volume in argo workflow