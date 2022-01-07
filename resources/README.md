### Creación de Pod
Pod: Esta compuesto por contenedores Docker
nginx sera la imagen en la cual se basa el pod
```
Kubectl run pod-nginx --image=nginx
```
Para consultar, comprobar el estado de los pods que estén ejecutandose
```
Kubectl get nodes -o wide
```
### Describe
Con **Describe** podemos ver todas las propiedades de cualquier objeto
```
Kubectl describe pod/pod-nginx
```
### Exec
Con **Exec** nos permite ejecutar un comando contra un contenedor, es decir para el ejemplo: ejecuta el comando **ls** dentro del pod-nginx
```
Kubectl get pods

Kubectl exec pod-nginx ls
```
### -it bash
Para introducirme dentro del contenedor: **-it bash**
```
Kubectl exec pod-nginx -it bash
```
**uname** para conocer la versión del kernel que está ejecutando el host
```
Uname -a
```
### df -h
Para comprobar los sistemas de ficheros
```
df -h
```
### run
Crear un pod con apache y asignarle el puerto 8080
```
Kubectl run apache --image=httpd --port=8080
```
### logs
Para ver las 30 ultimas lineas que tengo disponibles en el log de pod-nginx
```
kubectl logs apache
kubectl logs pod-nginx --tail=30
```
Para introducirme dentro del contenedor apache: **-it bash**
```
Kubectl exec apache -it bash
```

### wget
Usando al comando wget
```
wget localhost
```
### apt-get update
Pero no esta disponble, entonces lo primero es actualizar el container con el siguiente comando
```
apt-get update
```

### Instalar el comando **wget**
```
apt-get install wget
```

### wget localhost
Ahora si vamos a probar wget localhost
```
wget localhost
```

### logs -f
Entonces para mirar el log de cualquier pod en modo depuración seria con el comando logs -f
```
kubectl logs -f apache
```

### docker ps
Buscar en docker un contenedor con la palabra "apache"
```
docker ps | findstr "apache"
```

### create
Crear un pod a travez de una especificación en YAML
```
Kubectl create -f nginx.yaml
```

### delete
Borrar un Pod
```
kubectl delete pod/nginx1 
```

### describe
Obtener el detalle de un pod
```
kubectl describe pod/nginx
```


### get pod nginx -o yaml
Obtener el detalle de un pod en formato YAML
```
kubectl get pod nginx -o yaml 
```

### get pod nginx -o yaml >nginx-file.yaml
Obtener el detalle de un pod en formato YAML y guardarlo en un fichero nginx-file.yaml
```
kubectl get pod nginx -o yaml >nginx-file.yaml
```

### delete pod apache --grace-period=5
Borrar el pod apache con un periodo de espera de 5 segundos
```
kubectl delete pod apache --grace-period=5 
```

### delete pod apache --now
Borrar el pod apache inmediatamente
```
kubectl delete pod apache --now
```

### delete pods --all
Borrar todos los pods
```
kubectl delete pods --all 
```

## create -f .\multi.yaml
Pod con multiples contenedores
Crear un Pod con multi contenedores a partir de un fichero YAML
```
kubectl create -f .\multi.yaml
```

### logs pod/redis-django -c almacen
Para ver el log de uno de los contenedores de un Pod, para este ejemplo: almacen
```
kubectl logs pod/redis-django -c almacen
```

### apply 
De los unicos comandos para trabajar de forma declarativa
Primero vamos a borrar todos los pods que tenemos
```
kubectl delete pods --all 
```

### create -f .\nginx.yaml
Crear pod nginx a travez de un fichero YAML
```
kubectl create -f .\nginx.yaml
```

### describe pod nginx
Ver el detalle de un pod nginx
```
kubectl describe pod nginx
```

### get pod nginx -o yaml
Obtener informacion de un pod en formato YAML
```
kubectl get pod nginx -o yaml
```

### delete pod/nginx
Borrar pod/nginx
```
kubectl delete pod/nginx 
```

### apply -f .\nginx.yaml
"**apply**" para crear un pod a partir de un fichero YAML
```
kubectl apply -f .\nginx.yaml 
```

### Agregar una etiqueta nueva a mi fichero YAML
```
labels:
    zone: prod
    version: v1
    other: myLabel
```

### get pod nginx -o yaml
Obtener la información del pod y vemos que ha cambiado el estado, con la nueva etiqueta
```
kubectl get pod nginx -o yaml
```

## Restart policy 
- **Always**: siempre que el pod sufra una caida, que reinicie (por dafult todos lo tienen)
- **OnFailure**: Reinicia el Pod solo si ha fallado
- **Never**: nunca se reinicie

### apply -f restart-always.yaml
Crear un pod con una politica de reinicio
```
kubectl apply -f restart-always.yaml
```

### describe
El pod tiene Cero reinicios -> Restart Count: 0 
```
kubectl describe pod/tomcat
```

### exec -it tomcat bash
Entrar a la shell del contenedor
```
Kubectl exec -it tomcat bash
```

### Parar tomcat mediente **catalina.sh stop**
Este seria el trace que me devuelve:
```
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-11
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
root@tomcat:/usr/local/tomcat# command terminated with exit code 137
```

### get pods
Consultar que se ha reiniciado el pod de tomcat nuevamente --> restartCount: 1
```
kubectl get pods
```

### delete
```
Kubectl delete pod/tomcat
```

### restartPolicy: OnFailure 
Aplicar politica de **OnFailure**
```
restartPolicy: OnFailure
```
### get pod nginx -o jsonpath='{.status.containerStatuses[0].containerID}'
Obtener el ID del primer contenedor en el Pod
```
kubectl get pod nginx -o jsonpath='{.status.containerStatuses[0].containerID}'
```

### get pods -o wide
Obtener mas informacion al consultar los Pods
```
kubectl get pods -o wide
```

## Labels
Obtener la informacion de las labels
```
 kubectl get pod tomcat --show-labels
 ```

El resultado es el siguiente:
```
NAME     READY   STATUS    RESTARTS   AGE    LABELS
tomcat   1/1     Running   0          2m9s   estado=desarrollo
```


Pintar una columna estado al hacer la consulta de las labels
```
kubectl get pod tomcat --show-labels -L estado
```
El resultado es el siguiente:
```
NAME     READY   STATUS    RESTARTS   AGE     ESTADO       LABELS
tomcat   1/1     Running   0          2m42s   desarrollo   estado=desarrollo
```

### apply -f .
Aplicar todos los ficheros que estan en un directorio
```
kubectl apply -f .
```
El resultado es el siguiente:
```
pod/tomcat configured
pod/tomcat1 created
pod/tomcat2 created
pod/tomcat3 created
```

### get pods --show-labels
Obtener las etiquetas de todos los pods
```
kubectl get pods --show-labels
```

### get pods --show-labels -l estado=desarrollo
Obtener todos los pods cuya etiqueta sea "desarrollo"
```
kubectl get pods --show-labels -l estado=desarrollo
```

### get pods --show-labels -l estado=desarrollo,responsable=juan
Obtener todos los pods cuya etiquetas sea "estado=desarrollo" y "responsable=juan"
``` 
kubectl get pods --show-labels -l estado=desarrollo,responsable=juan
``` 

### get pods --show-labels -l estado!=desarrollo,responsable!=juan
Obtener todos los pods cuya etiquetas NO sean  "estado=desarrollo" y "responsable=juan"
``` 
kubectl get pods --show-labels -l estado!=desarrollo,responsable!=juan 
```

### get pods --show-labels -l estado!=testing
Obtener todos los pods cuya etiquetas NO sea  "estado=testing" 
```
kubectl get pods --show-labels -l estado!=testing
```

### get pods --show-labels -l 'estado in(desarrollo)'
Obtener todos los pods cuya etiqueta este en "desarrollo" 
```
kubectl get pods --show-labels -l 'estado in(desarrollo)'
```

### get pods --show-labels -l 'estado in(desarrollo, testing)'
Obtener todos los pods cuya etiqueta este en "desarrollo" y "testing" 
```
kubectl get pods --show-labels -l 'estado in(desarrollo, testing)'
```

### get pods --show-labels -l 'estado notin(desarrollo, testing)' 
Obtener todos los pods cuya etiqueta NO este en "desarrollo" y "testing"
```
kubectl get pods --show-labels -l 'estado notin(desarrollo, testing)' 
```

### delete pods -l 'estado in(desarrollo)'   
Borrar todos los pods cuya etiqueta este en "desarrollo"
```
kubectl delete pods -l 'estado in(desarrollo)'   
```
El resultado es el siguiente:

```
pod "tomcat" deleted
pod "tomcat1" deleted
```

### get pod tomcat4 -o jsonpath="{.metadata.annotations}"
Obtener anotaciones de un pod mediante formato json
```
kubectl get pod tomcat4 -o jsonpath="{.metadata.annotations}"
```

## Deployment
Un **deployment** es un wrapper que proporciona actualizaciones declarativas para los Pods y los ReplicaSets.

### create deployment apache --image=httpd
Crear un deployment en modo Run (imperativo)
```
kubectl create deployment apache --image=httpd
```

### get deploy
Para ver los deplyments
```
kubectl get deploy    
```

### get rs
Para consultar los ReplicaSets
```
kubectl get rs
```    

### describe deploy
Obtener el detalle de un deploy apache
```
kubectl describe deploy apache
```

### Crear un deployment mediante un YAML
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2 # Indica al controlador que ejecute  2 pods
  selector: # permite seleccionar un conjunto de objetos que cumplan las condiciones
    matchLabels:
      app: nginx
  template: # Plantilla que define los containers
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

### apply -f .\deploy_nginx.yaml
Aplicar un despliegue mediante un fichero YAML
De forma que lo puedo modificar rapidamente sin reconstruir el objeto
```
kubectl apply -f .\deploy_nginx.yaml
```
Aparece lo siguiente:
deployment.apps/nginx-d created

### get deploy
Obtener información de los despliegues
```
kubectl get deploy
```

### get rs 
Obtener los replicasets
```
kubectl get rs 
```

### get rs -o wide
Obtener el detalle de los replicasets 
```
kubectl get rs -o wide
```

### get deploy -o wide 
Obtener el detalle de los despliegues
```
kubectl get deploy -o wide 
```

### get pods -l app=nginx
Buscar todos los pods con la etiqueta = nginx
```
kubectl get pods -l app=nginx
```

### get pods -l app=nginx -L app
Obtener todos los pods con la etiqueta nginx y colocar una columna "app"
```
kubectl get pods -l app=nginx -L app
```

### get pods -L app
Obtener todos los pods y agregar una columna "app"
```
kubectl get pods -L app
```

### get pods,deploy,rs
Consultar en una sola linea Pods, Deployments, y Replicasets
```
kubectl get pods,deploy,rs  
```

### get pods,deploy,rs -l app=nginx
Consultar en una sola linea Pods, Deployments, y Replicasets cuya etiqueta es "nginx"
```
kubectl get pods,deploy,rs -l app=nginx
```

### edit deploy nginx-d
Editar en caliente el fichero de despliegue y modificar informacion
```
kubectl edit deploy nginx-d
```

### get deploy nginx-d -o yaml >deploy_nginx-file.yaml
Obtener el fichero de despliegue y guardarlo en un fichero YAML
```
 kubectl get deploy nginx-d -o yaml >deploy_nginx-file.yaml 
```

### scale deploy nginx-d --replicas=5
Escalar con 5 replicas el deployment de nginx-d
```
kubectl scale deploy nginx-d --replicas=5
```



