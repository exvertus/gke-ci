#### The contents of this folder was created with the following commands:
1 `helm repo update`
1 `helm fetch --untar --untardir [CHART_FOLDER] stable/jenkins`
1 `helm template base --output-dir git/gke-ci/bases/helm-fetch-template/ [CHART_FOLDER]/jenkins`

When this was run, it also spit out the test Pod to stdout:

`
helm template base --output-dir git/gke-ci/bases/helm-fetch-template/ charts/jenkins
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/service-account.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/secret.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/config.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/jcasc-config.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/tests/test-config.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/home-pvc.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/rbac.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/rbac.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/rbac.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/rbac.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/jenkins-agent-svc.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/jenkins-master-svc.yaml
wrote git/gke-ci/bases/helm-fetch-template//jenkins/templates/jenkins-master-deployment.yaml
---
/# Source: jenkins/templates/tests/jenkins-test.yaml
apiVersion: v1
kind: Pod
metadata:
  name: "base-ui-test-ow3hq"
  namespace: default
  annotations:
    "helm.sh/hook": test-success
spec:
  initContainers:
    - name: "test-framework"
      image: "dduportal/bats:0.4.0"
      command:
      - "bash"
      - "-c"
      - |
        set -ex
        # copy bats to tools dir
        cp -R /usr/local/libexec/ /tools/bats/
      volumeMounts:
      - mountPath: /tools
        name: tools
  containers:
    - name: base-ui-test
      image: jenkins/jenkins:lts
      command: ["/tools/bats/bats", "-t", "/tests/run.sh"]
      volumeMounts:
      - mountPath: /tests
        name: tests
        readOnly: true
      - mountPath: /tools
        name: tools
  volumes:
  - name: tests
    configMap:
      name: base-jenkins-tests
  - name: tools
    emptyDir: {}
  restartPolicy: Never
`