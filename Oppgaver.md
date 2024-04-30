
# Oppgave 1

Lag et helm chart med 
```sh
$ helm create [name...]
```
Og deretter kjør
```
$ cd [navn til helm chart]
```

# Oppgave 2

her skal du redigere din "values.yaml" fil med editor of choice

Se etter denne og
Endre pull policy til "Always"
```yaml
image:
  repository: nginx
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: ""
  ```
Deretter endre "nameOverride" til hva du vil
og legg til et service account name (anbefalt med samme navn som mappe)
```yaml
imagePullSecrets: []
nameOverride: "Hello-App"
fullnameOverride: ""

serviceAccount:
  # Specifies whether a service account should be created
  create: true
  # Automatically mount a ServiceAccount's API credentials?
  automount: true
  # Annotations to add to the service account
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name: "hello"
```
Deretter skal du endre Service fra type ClusterIP til "NodePort"
```yaml
service:
  type: ClusterIP
  port: 80
```
# Oppgave 3, Deploy cluster

For å deploye din K8S cluster må du installere helm på den for å ordne dependencies
```sh
$ helm install --generate-name [chartmappen]/ --values [chartmappen]/values.yaml
```

Og kjør kommandoene som står etter
Eksemel på hvordan det skal se ut 
```sh
LAST DEPLOYED: Tue Apr 30 08:54:19 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=fredted,app.kubernetes.io/instance=fredted-1714460059" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
```

Og hvis du kjører
```sh
$ minikube kubectl get pod
```
Så burde du kunne se noden du lagde nettop