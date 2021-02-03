## About

This repo accompanies a training course hosted on the Pluralsight skills platform, called **Automating Kubernetes Deployments Using a GitOps Workflow**. It contains deployment configuration designed for use with the Flux, Helm and Flagger operators.

f install --namespace flux --git-url git@github.com:migspedroso/gitops-nginxhello.git --git-user migpedroso --git-email luizmpf@gmail.com | k apply -f -

k exec flux-76d9954774-hlh2t -- ls /etc/fluxd/ssh

export FLUX_FORWARD_NAMESPACE=flux

f identity

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDLu9H9aX0i8oZiHVXIAH78f4xjK+6wAeYP8Vop04z6BIv5zlLYg/QTfgoIoS+naXjkJjXanej9JHVzgeJFu6boZ4WFzuSvFQ3M4z4SXbsHNG6h/a4qGJ7gGdsCn7JDLlkujTiSTGNaeIZNbD2R7YgOxn1EVj92orZQCgrAIxRtC0SoGHyXTwvPE5iQpkNWwkLOjgp2/a4KREUJZQrv+zudqrwASsriyHH3eO8rNeblxE1DribGwvidNF/jGqKGC9ytnmBcLDc9JwldJxhXc7bjyvDk+k6R2I36/TwoZkok+DzOWdcuk+TNe/gzNS9NHrB9L8g4RRb6k9cQ3vQdZSR20DP6I1w83DWNTWdLCgJSxb2OwiXY+EblEhpklke0kT4POoVT2P9we/DhoTs3gmgta3vHs0obdxxceQyfzX181PTm1asas2BNmZYFTOCsTHzykATwHs5T/dKKlSyR4QeKOCIGAdXb8f+fxTUXgx7XCXm8Vmdp4f1T+dg3T5ZmAgM= root@flux-76d9954774-hlh2t

f policy -w=default:deployment/nginxhello --tag='nginxhello=semver:~1.19.x'
f policy -w=default:deployment/nginxhello --automate
k scale --replicas=1 deployment nginxhello

k apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/1.2.0/deploy/crds.yaml

helm repo add fluxcd https://charts.fluxcd.io

helm install helm-operator fluxcd/helm-operator -n flux --set helm.versions=v3 --set git.ssh.secretName=flux-git-deploy 

k get all -n flux -l app=helm-operator