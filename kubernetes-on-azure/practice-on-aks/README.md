### az aks install-cli
Instalar az aks install-cli
```
az aks install-cli --help | more
```
### az account show
Obtiene la informacion de la suscripción
```
az account show
```
### az configure --defaults group
Establece el resource group por defecto, para este ejemplo: "aks-gr1"
```
az configure --defaults group=aks-rg1
```
###  kubectl config current-context
Para obtener el contexto actual donde trabajo
```
 kubectl config current-context
```
###  kubectl get nodes
Para obtener los nodes
```
 kubectl get nodes
```
###  kubectl create deployment
Create deployment a partir de una imagen ubicada en docker registry
```
 kubectl create deployment myapp --image=eduflornet/myapp:v1 --replicas=1
```
###  kubectl get deployments
Consultar los deployments
```
 kubectl get deployments
```
###  kubectl expose deployment
Crear un servicio de tipo balanceado (puerto 80)
```
 kubectl expose deployment myapp --type=LoadBalancer
```
###  kubectl expose deployment
Crear un servicio de tipo balanceado (puerto 80)
```
 kubectl expose deployment myapp --type=LoadBalancer
```
###  kubectl expose deployment
Consulta los eventos en los servicios, puertos, IP, external IP
```
 kubectl get svc --watch
```
###  kubectl scale deployment
Escala un servivio desplegado a 3 replicas, por ejemplo:
```
 kubectl scale deployment myapp --replicas=3
```
