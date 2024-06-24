# grafana_as_code
ArgoCD + Grafana bits and pieces for testing

Testing ArgoCD + Grafana Cloud for declarative approaches. 

Required: 

Grafana Cloud account 
https://grafana.com/get/


Kubernetes Cluster or single node operation


lightweight and cheap to run:
https://k3s.io/

ephemeral and fast: https://kind.sigs.k8s.io/

OpenShift/OKD (Single node where possible, control plane + data plane is too power consuming for lab/testing. Save energy! Multi node for prod/infra dev and testing)

https://docs.okd.io/latest/installing/installing_sno/install-sno-installing-sno.html

The Grafana Operator

https://github.com/grafana/grafana-operator


Installation and Some knowledge of ArgoCD

https://argo-cd.readthedocs.io/en/stable/

Only the Dashboards directory needs to be sync'd at this time. The manifests folder is a place holder. **DO NOT STORE SECRETS IN GIT!!**


