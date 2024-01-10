# Challenge

The infrastructure is set up with minikube and fluxcd.
The bitnami PostgreSQL HA chart is used for a highly available PostgreSQL
database backend.  
The app consists of two parts: an import job for PostgreSQL
and the HA API deployment with the /success endpoint and a ReplicaSet of 2.
  
- Database: PostgresqlHA  
- Import: gtfso-import  
- API: gtfso-vbb  
  Monitoring: kube-prometheus-stack
  Vulnerability Scanning: Trivy

## Clone repository
```
$ git clone https://git.e2m.io/mue/obch
$ cd obch
```

## Setup minikube and flux
### Optionally remove remnants of previous minikube clusters
```
$ minikube delete --all

# The above was not sufficient to setup a new cluster
# See also: https://github.com/kubernetes/minikube/issues/17683
# Additionally deleting the local minikube config folder helped:
$ rm -rf ~/.minikube
```

### Setup cluster and deploy app
run sh sources 'setup-cluster' and 'deploy'
```
$ ./run.sh
```

### Setup cluster
```
$ ./setup-cluster
```

## Deploy Service
```
$ ./deploy
```
### Stop cluster
```
$ minikube stop
```

## TODOs / Notes
gtfso-import needs the database secret for import  
Vulnerability scanning in github action with https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning   
Add monitoring target to kube-prometheus-stack  
Define strategy for version updates  
Consider SOPS for secret management  
Terraform has minikube and flux providers  

## Resources
[Flux bootstrap for Gitea](https://fluxcd.io/flux/installation/bootstrap/gitea/)  
[Flux github action](https://fluxcd.io/flux/flux-gh-action/)  
[Flux Monitoring](https://github.com/fluxcd/flux2-monitoring-example)  
[Terraform Flux Provider](https://github.com/fluxcd/terraform-provider-flux)  
[Mozilla SOPS](https://fluxcd.io/flux/guides/mozilla-sops/)  
[bitnami PostgreSQL HA Helm](https://bitnami.com/stack/postgresql-ha/helm)  
[Trivy](https://github.com/aquasecurity/trivy)
