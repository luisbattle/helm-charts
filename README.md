# Helm example

## Deploy
Ejecutar scripts:

    backend_deploy.sh
    frontend_deploy.sh

Se deployan los servicios en los namespaces frontend y backend

## Levantar tunnel minikube
Minikube expone el LB para que el FE pueda conectarse al BE, ejecutar `minikube tunnel`
    FE:3000   --->  BE(127.0.0.1:3001)

## FLUX-CD

LINK: https://github.com/fluxcd/helm-operator-get-started

helm upgrade -i flux luisbattle/flux --wait \
--namespace fluxcd \
--set git.url=git@github.com:luisbattle/helm-operator-get-started

Genera la ssh-key()
kubectl -n fluxcd logs deployment/flux | grep identity.pub | cut -d '"' -f2

Install helm Operator

helm upgrade -i helm-operator fluxcd/helm-operator --wait \
--namespace fluxcd \
--set git.ssh.secretName=flux-git-deploy \
--set helm.versions=v3