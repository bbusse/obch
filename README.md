# Challenge

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

## TODOs / Notes
Define strategy for version updates  
Use SOPS for secret management  
Terraform has minikube and flux providers  

## Resources
[Flux bootstrap for Gitea](https://fluxcd.io/flux/installation/bootstrap/gitea/)  
[Flux github action](https://fluxcd.io/flux/flux-gh-action/)  
[Terraform Flux Provider](https://github.com/fluxcd/terraform-provider-flux)  
[Mozilla SOPS](https://fluxcd.io/flux/guides/mozilla-sops/)  
[bitnami PostgreSQL HA Helm](https://bitnami.com/stack/postgresql-ha/helm)  
