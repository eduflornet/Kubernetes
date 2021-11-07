# Notas

Pod: Esta compuesto por contenedores Docker
### Creación de Pod
nginx sera la imagen en la cual se basa el pod

```
Kubectl run pod-nginx --image=nginx
```
### Para consultar, comprobar el estado de los pods que estén ejecutandose
```
Kubectl get nodes -o wide
```

### Con **Describe** podemos ver todas las propiedades de cualquier objeto
```
Kubectl describe pod/pod-nginx
```

### Con **Exec** nos permite ejecutar un comando contra un contenedor, es decir para el ejemplo: ejecuta el comando **ls** dentro del pod-nginx
```
Kubectl get pods

Kubectl exec pod-nginx ls
```

### Para introducirme dentro del contenedor: **-it bash**
```
Kubectl exec pod-nginx -it bash
```
**uname** para conocer la versión del kernel que está ejecutando el host
```
Uname -a
```

### Para comprobar los sistemas de ficheros
```
df -h
```

### Crear un pod con apache y asignarle el puerto 8080
```
Kubectl run apache --image=httpd --port=8080

kubectl logs apache
```

### Para ver las 30 ultimas lineas que tengo disponibles en el log de pod-nginx
```
kubectl logs pod-nginx --tail=30
```

###  Para introducirme dentro del contenedor apache: **-it bash**
```
Kubectl exec apache -it bash
```

### Usando al comando wget
```
wget localhost
```

Pero no esta disponble, entonces lo primero es actualizar el container con el siguiente comando
```
apt-get update
```

### Instalar el comando **wget**
```
apt-get install wget
```

### Ahora si vamos a probar wget localhost
```
wget localhost
```
### Entonces para mirar el log de cualquier pod en modo depuración seria con el comando logs -f
```
kubectl logs -f apache
```

### Para buscar en docker un contenedor con la palabra "apache"
```
docker ps | findstr "apache"
```

### Crear un pod a travez de una especificación en YAML
```
Kubectl create -f nginx.yaml
```

### Borrar un Pod
```
kubectl delete pod/nginx1 
```

### Obtener el detalle de un pod
```
kubectl describe pod/nginx
```

### Obtener el detalle de un pod en formato YAML
```
kubectl get pod nginx -o yaml 
```

### Obtener el detalle de un pod en formato YAML y guardarlo en un fichero nginx-file.yaml
```
kubectl get pod nginx -o yaml >nginx-file.yaml
```

### Borrar el pod apache con un periodo de espera de 5 segundos
```
kubectl delete pod apache --grace-period=5 
```

### Borrar el pod apache inmediatamente
```
kubectl delete pod apache --now
```

### Borrar todos los pods
```
kubectl delete pods --all 
```

## Pod con multiples contenedores
### Crear un Pod con multi contenedores a partir de un fichero YAML
```
kubectl create -f .\multi.yaml
```

### Para ver el log de uno de los contenedores de un Pod, para este ejemplo: almacen
```
kubectl logs pod/redis-django -c almacen
```

## Comando apply - son de los unicos para trabajar de forma declarativa
### Primero vamos a borrar todos los pods que tenemos
```
kubectl delete pods --all 
```

### Crear pod nginx a travez de un fichero YAML
```
kubectl create -f .\nginx.yaml
```

### Ver el detalle de un Pod nginx
```
kubectl describe pod nginx
```

### Obtener informacion de un pod en formato YAML
```
kubectl get pod nginx -o yaml
```

### Borrar pod/nginx
```
kubectl delete pod/nginx 
```

### Usando comando "**apply**" para crear un pod a partir de un fichero YAML
```
kubectl apply -f .\nginx.yaml 
```

### Supongamos que agrego una etiqueta nueva a mi fichero YAML
```
labels:
    zone: prod
    version: v1
    other: myLabel
```

### Ahora vuelvo a obtener la información del pod y vemos que ha cambiado el estado, con la nueva etiqueta
```
kubectl get pod nginx -o yaml
```

## Restart policy 
### **Always**: siempre que el pod sufra una caida, que reinicie (por dafult todos lo tienen)
### **OnFailure**: Reinicia el Pod solo si ha fallado
### **Never**: nunca se reinicie

### Voy a crear un pod con una politica de reinicio
```
kubectl apply -f restart-always.yaml
```

### Observa que el pod tiene Cero reinicios -> Restart Count: 0 
```
kubectl describe pod/tomcat
```

### Voy a entrar a la shell del contenedor
```
Kubectl exec -it tomcat bash
```

### Voy a parar tomcat mediente **catalina.sh stop**
### Observa que me ha sacado del contenedor
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

### Ahora consultamos y observa que se ha reiniciado el pod de tomcat nuevamente --> restartCount: 1
```
kubectl get pods
```

### Ahora lo borramos
```
Kubectl delete pod/tomcat
```

### Aplicamos una politica de **OnFailure**
```
restartPolicy: OnFailure
```
### Obtener el ID del primer contenedor en el Pod
```
kubectl get pod nginx -o jsonpath='{.status.containerStatuses[0].containerID}'
```

### Obtener mas informacion al consultar los Pods
```
kubectl get pods -o wide
```

## Labels
### Obtener la informacion de las labels
```
 kubectl get pod tomcat --show-labels
 ```

El resultado es el siguiente:
```
NAME     READY   STATUS    RESTARTS   AGE    LABELS
tomcat   1/1     Running   0          2m9s   estado=desarrollo
```


### Pintar una columna estado al hacer la consulta de las labels
```
kubectl get pod tomcat --show-labels -L estado
```
El resultado es el siguiente:
```
NAME     READY   STATUS    RESTARTS   AGE     ESTADO       LABELS
tomcat   1/1     Running   0          2m42s   desarrollo   estado=desarrollo
```

### Aplicar todos los ficheros que estan en un directorio
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

### Obtener las etiquetas de todos los pods
```
kubectl get pods --show-labels
```

### Obtener todos los pods cuya etiqueta sea "desarrollo"
```
kubectl get pods --show-labels -l estado=desarrollo
```

### Obtener todos los pods cuya etiquetas sea "estado=desarrollo" y "responsable=juan"
``` 
kubectl get pods --show-labels -l estado=desarrollo,responsable=juan
``` 

### Obtener todos los pods cuya etiquetas NO sean  "estado=desarrollo" y "responsable=juan"
``` 
kubectl get pods --show-labels -l estado!=desarrollo,responsable!=juan 
```

### Obtener todos los pods cuya etiquetas NO sea  "estado=testing" 
```
kubectl get pods --show-labels -l estado!=testing
```

### Obtener todos los pods cuya etiqueta este en "desarrollo" 
```
kubectl get pods --show-labels -l 'estado in(desarrollo)'
```

### Obtener todos los pods cuya etiqueta este en "desarrollo" y "testing" 
```
kubectl get pods --show-labels -l 'estado in(desarrollo, testing)'
```

### Obtener todos los pods cuya etiqueta NO este en "desarrollo" y "testing"
```
kubectl get pods --show-labels -l 'estado notin(desarrollo, testing)' 
```

### Borrar todos los pods cuya etiqueta este en "desarrollo"
```
kubectl delete pods -l 'estado in(desarrollo)'   
```
El resultado es el siguiente:

```
pod "tomcat" deleted
pod "tomcat1" deleted
```

### Obtener anotaciones de un pod mediante formato json
```
kubectl get pod tomcat4 -o jsonpath="{.metadata.annotations}"
```

## Deployment
### Un **deployment** es un wrapper que proporciona actualizaciones declarativas para los Pods y los ReplicaSets.

### Crear un deployment en modo Run (imperativo)
```
kubectl create deployment apache --image=httpd
```

### Para ver los deplyments
```
kubectl get deploy    
```

### Para consultar los ReplicaSets
```
kubectl get rs
```    

### Obtener el detalle de un deploy apache
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
