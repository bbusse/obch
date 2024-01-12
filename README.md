# Challenge

The infrastructure is set up with minikube and fluxcd.  
The bitnami PostgreSQL HA chart is used for a highly available PostgreSQL
database backend.  
kube-prometheus-stack is used for monitoring.  
Trivy scans for vulnerabilities.  
  
The app consists of two parts: an import job for PostgreSQL
and the HA API deployment with the /success endpoint and a ReplicaSet of 2.
  
- Database: PostgresqlHA  
- Import: gtfso-import  
- API: gtfso-vbb  
  Monitoring: kube-prometheus-stack  
  Vulnerability Scanning: Trivy

## Clone repository
```
$ git clone https://github.com/bbusse/obch
$ cd obch
```

## Setup minikube and flux
### Optionally remove remnants of previous minikube clusters
```
$ minikube delete --all

# The above was not sufficient to setup a new cluster
# Additionally deleting the local minikube config folder helped:
$ rm -rf ~/.minikube
```

### Setup cluster and deploy app
run sh sources 'setup-cluster' and 'deploy'
> [!NOTE]
> A Personal Access Token (PAT) is needed for fluxcd
> to create and/or write to its state repository
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

## Expected Result
Probe APIs /success endpoint
```
$ kubectl port-forward --namespace app gtfso-vbb-8586b6cddc-f29bh 8080:5000 &
$ curl http://localhost:8080/success
Success!
```
Probe Metrics
```
$ curl http://localhost:8080/metrics
```
Show services
```
 kubectl get service -A                                                                                                                      NAMESPACE       NAME                                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
app             gtfso-vbb                                            ClusterIP   10.99.63.60      <none>        80/TCP                         31m
app             pgsql-ha-postgresql-ha-pgpool                        ClusterIP   10.96.68.65      <none>        5432/TCP                       3h23m
app             pgsql-ha-postgresql-ha-postgresql                    ClusterIP   10.101.13.69     <none>        5432/TCP                       3h23m
app             pgsql-ha-postgresql-ha-postgresql-headless           ClusterIP   None             <none>        5432/TCP                       3h23m
default         kubernetes                                           ClusterIP   10.96.0.1        <none>        443/TCP                        3h31m
flux-system     notification-controller                              ClusterIP   10.111.215.54    <none>        80/TCP                         3h27m
flux-system     source-controller                                    ClusterIP   10.107.64.201    <none>        80/TCP                         3h27m
flux-system     webhook-receiver                                     ClusterIP   10.109.54.68     <none>        80/TCP                         3h27m
kube-system     kube-dns                                             ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP         3h31m
kube-system     prometheus-kube-prometheus-coredns                   ClusterIP   None             <none>        9153/TCP                       3h10m
kube-system     prometheus-kube-prometheus-kube-controller-manager   ClusterIP   None             <none>        10257/TCP                      3h10m
kube-system     prometheus-kube-prometheus-kube-etcd                 ClusterIP   None             <none>        2381/TCP                       3h10m
kube-system     prometheus-kube-prometheus-kube-proxy                ClusterIP   None             <none>        10249/TCP                      3h10m
kube-system     prometheus-kube-prometheus-kube-scheduler            ClusterIP   None             <none>        10259/TCP                      3h10m
kube-system     prometheus-kube-prometheus-kubelet                   ClusterIP   None             <none>        10250/TCP,10255/TCP,4194/TCP   3h8m
monitoring      alertmanager-operated                                ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP     3h8m
monitoring      prometheus-grafana                                   ClusterIP   10.105.202.74    <none>        80/TCP                         3h10m
monitoring      prometheus-kube-prometheus-alertmanager              ClusterIP   10.110.246.137   <none>        9093/TCP,8080/TCP              3h10m
monitoring      prometheus-kube-prometheus-operator                  ClusterIP   10.105.238.213   <none>        443/TCP                        3h10m
monitoring      prometheus-kube-prometheus-prometheus                ClusterIP   10.102.52.55     <none>        9090/TCP,8080/TCP              3h10m
monitoring      prometheus-kube-state-metrics                        ClusterIP   10.106.92.254    <none>        8080/TCP                       3h10m
monitoring      prometheus-operated                                  ClusterIP   None             <none>        9090/TCP                       3h8m
monitoring      prometheus-prometheus-node-exporter                  ClusterIP   10.109.107.194   <none>        9100/TCP                       3h10m
security-scan   trivy-trivy-operator                                 ClusterIP   None             <none>        80/TCP                         3h6m
```

## TODOs / Notes
gtfso-import needs the database secret for import  
gtfso-import: Retry job until success  
Add gtfs-vbb as target to prometheus  
Change default credentials for the kube-prometheus-stack  
Define strategy for version updates  
Consume and act on Trivy results  
Consider SOPS / Vault for secret management  
Terraform has minikube and flux providers  
  
For a pure GitOps experience the path containing the yaml manifests 
created by 'flux create [..] --export' would have to be added to the fluxcd
repository

## Resources
[Flux bootstrap for Gitea](https://fluxcd.io/flux/installation/bootstrap/gitea/)  
[Flux github action](https://fluxcd.io/flux/flux-gh-action/)  
[Flux Monitoring](https://github.com/fluxcd/flux2-monitoring-example)  
[Flux Monotioring custom metrics](https://fluxcd.io/flux/monitoring/custom-metrics/)  
[Terraform Flux Provider](https://github.com/fluxcd/terraform-provider-flux)  
[Mozilla SOPS](https://fluxcd.io/flux/guides/mozilla-sops/)  
[bitnami PostgreSQL HA Helm](https://bitnami.com/stack/postgresql-ha/helm)  
[Trivy](https://github.com/aquasecurity/trivy)
