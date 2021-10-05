# Helm example

## Deploy
Ejecutar scripts:

    backend_deploy.sh
    frontend_deploy.sh

Se deployan los servicios en los namespaces frontend y backend

## Levantar tunnel minikube
Minikube expone el LB para que el FE pueda conectarse al BE, ejecutar ´minikube tunnel´
    FE:3000   --->  BE(127.0.0.1:3001)
