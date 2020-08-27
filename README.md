# gke-ci

GKE manifests for running continuous integration pipeline(s).

#### Low-cost cluster settings
* Location type: zonal
* Release channel: Regular channel
* Image type: Container-Optimized OS
* Enable autoscaling: 0-3
* Series: N1
* Machine type: g1-small

#### How to gank the Jenkins Helm chart as yaml
1. Install [Helm](https://helm.sh/docs/intro/install/)
1. Add stable repo: `helm repo add stable https://kubernetes-charts.storage.googleapis.com/`
1. Render chart to base: `helm template -n ci stable/jenkins > base/jenkins-helm.yaml`
Unless taking upstream changes, do not edit the contents of base. Use kustomization instead.

#### How to make kustomizations
