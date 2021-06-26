# Cluster Setup

* Location type: Zonal
* Default pool
  * Nodes: 1
  * At least e2-medium.
  * Security *Set access to each api* **MUST DO CLOUD PLATFORM**
  * Enable secure boot.
* Cluster
  * Security: Enable workload identity.
  * Features: Enable Compute Engine Persistent Disk CSI Driver.

## Configure kubectl access

`gcloud container clusters get-credentials `\
`kubectx hornet=gke_assembler-core_australia-southeast1-a_`

## Setup NGINX Ingress

Create admin role:

```
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole cluster-admin \
  --user $(gcloud config get-value account)
```

Apply the controller:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.41.2/deploy/static/provider/cloud/deploy.yaml
```

## Setup postgres:

### Create a PG secret to use:

First add the password (randomly generate them, then base64 encode) to the `manual_setup/postgres/secret.yaml`

`kubectl apply -f config/arch/manual_setup/postgres/secret.yaml`

### Install PG

Add the repo:\
`helm repo add bitnami `[`https://charts.bitnami.com/bitnami`](https://charts.bitnami.com/bitnami)

* Download the values file.
* Initially change pg repliacas to 1.
* Update the postgres image to the most recent, [see tags](https://hub.docker.com/r/bitnami/postgresql-repmgr/tags/?page=1&ordering=last_updated)
* Update the postgres existing secret to 'postgres'
* Turn on replica

`helm install postgres bitnami/postgresql --values config/arch/helm/postgres.yaml`

## Setup keycloak

* Log into the PG pod
* Log in as postgres `psql -U postgres`
* `CREATE DATABASE keycloak;`
* Make sure the included PG is disabled.

Add the extra envs, hardcode pw them then put them back to todo:

`helm install keycloak codecentric/keycloak --values config/arch/helm/keycloak.yaml`

### Keycloak Ingress:

Update host if needed.\
`kubectl apply -f config/arch/manual_setup/keycloak_ingress.yaml`

### Add keycloak admin:

* Log in to pod
* Jump to `/opt/jboss/keycloak/bin`
* `./add-user-keycloak.sh -r master -u keycloak -p test_1234`
* `./jboss-cli.sh --connect command=:reload`
* Log in, and change password.
* Set up OTP.

## Deploy Hornet

Make sure the container is build and pushed.\
Replace all todo values in the secret file, use rails c for this.

```
kubectl apply -f config/arch/hornet/secrets.yaml
```

Put todo values back.

Build the deployment:\
`kubectl apply -f config/arch/hornet/deployment.yaml`\
Build the service:\
`kubectl apply -f config/arch/hornet/service.yaml`\
Build the ingress:\
`kubectl apply -f config/arch/hornet/ingress.yaml`

Exec into the pod, create the db:

```
RAILS_ENV=production bin/rails db:create
RAILS_ENV=production bin/rails db:migrate
```

## SSL

Install cert manager:\
`kubectl apply -f `[`https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.yaml`](https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.yaml)\
Make sure you have three pods:\
`kubectl get pods --namespace cert-manager`

The following settings are gcp specific:\
Create service account:\
`gcloud iam service-accounts create dns01-solver --display-name "dns01-solver"`

```
gcloud projects add-iam-policy-binding  \
   --member serviceAccount:dns01-solver@.iam.gserviceaccount.com \
   --role roles/dns.admin
```

Link accounts:

```
gcloud iam service-accounts add-iam-policy-binding \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:.svc.id.goog[cert-manager/cert-manager]" \
    dns01-solver@.iam.gserviceaccount.com
```

```
kubectl annotate serviceaccount --namespace=cert-manager cert-manager \
    "iam.gke.io/gcp-service-account=dns01-solver@.iam.gserviceaccount.com"
```

Create the issuers:\
`kubectl apply -f config/arch/certs/dns_issuer.yaml`\
`kubectl apply -f config/arch/certs/http_issuer.yaml`

Create the cert:\
`kubectl apply -f config/arch/certs/core_cert.yaml`

Update the ingressess with:

```
  tls:
  - hosts:
    - dashboard.assembler.app
    secretName: assembler-app-tls
```

## Install Redis

Probably do it the same way as the waggle doc.

Add the sidekiq deployment:\
`kubectl apply -f config/arch/hornet/sidekiq.yaml`