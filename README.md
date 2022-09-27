**NOTE: This repo is part of [Hello OpenShift: Multi-Cluster GitOps].** It's
not intended to be referenced directly. Instead, check out the organization's
page for how this repository fits into the greater multi-cluster GitOps
architecture.

# GitOps Configurations for Stage Cluster

This repo contains Argo CD configurations for all applications on the stage
cluster. It is a Helm chart which deploys Namespaces, AppProjects, and
Applications, based on configurations in values.yaml.

This repo is continuously pushed by ACM to the stage cluster.

## Deploying

This will deploy an ACM Application (Subscription and other required objects)
on the hub cluster, which will continuously deploy this repo to the stage
cluster:

```bash
./deploy.sh
```

## Values

| Value                                        | Required? | Description |
| -------------------------------------------- | --------- | ----------- |
| .Values.appProjects                          | Yes       | List of projects to deploy |
| .Values.appProjects[].name                   | Yes       | Name of the Argo CD AppProject and OCP Project (namespace) applications will be deployed into |
| .Values.appProjects[].description            | Optional  | Description for the project. Default: "" |
| .Values.appProjects[].displayName            | Optional  | Display name for the project. Default: "" |
| .Values.appProjects[].gitUrl                 | Yes       | URL to the Git repo with application Helm charts for this project. NOTE: This URL should end in ".git". |
| .Values.appProjects[].gitBranch              | Optional  | Git branch to pull charts from. Default: "main" |
| .Values.appProjects[].valuesFile             | Optional  | Helm values file to use relative to each helm chart. Default: "values.yaml" |
| .Values.appProjects[].applications           | Yes       | List of applications to deploy in the project |
| .Values.appProjects[].applications[].name    | Yes       | Name of the application being deployed. NOTE: THIS MUST BE UNIQUE! Even if deployed into a different project, Argo CD Application names must be unique. |
| .Values.appProjects[].applications[].gitPath | Yes       | Path inside the project gitUrl to the Helm chart. Use "." if the chart is in the root of the repo. Use a relative path otherwise. |

[Hello OpenShift: Multi-Cluster Management]: https://github.com/hello-openshift-multicluster-gitops
