## Kubernetes

### ¿Qué es Kubernetes?

Kubernetes es una plataforma de código abierto para automatizar la implementación, escalado y operación de aplicaciones en contenedores. Kubernetes permite a los desarrolladores y administradores de sistemas gestionar aplicaciones de forma eficiente y escalable.

### Componentes de Kubernetes

Kubernetes consta de varios componentes que trabajan juntos para proporcionar una plataforma de orquestación de contenedores. Algunos de los componentes más importantes de Kubernetes son:

- *Kubelet*: El Kubelet es un agente que se ejecuta en cada nodo del clúster y se encarga de gestionar los contenedores en ese nodo.
- *Kube-proxy*: El Kube-proxy es un proxy de red que se ejecuta en cada nodo del clúster y se encarga de enrutar el tráfico de red a los contenedores.
- *Kube-controller-manager*: El Kube-controller-manager es un controlador que se encarga de gestionar los controladores de Kubernetes, como los controladores de replicación y los controladores de estado.
- *Kube-scheduler*: El Kube-scheduler es un componente que se encarga de asignar los pods a los nodos del clúster en función de los recursos disponibles.
- *Etcd*: Etcd es un almacén de datos distribuido que se utiliza para almacenar la configuración del clúster de Kubernetes.


### Ventajas de Kubernetes

- *Escalabilidad*: Kubernetes permite escalar aplicaciones de forma eficiente y automática.
- *Despliegue continuo*: Kubernetes facilita el despliegue continuo de aplicaciones al proporcionar una plataforma de orquestación de contenedores.
- *Gestión de recursos*: Kubernetes permite gestionar eficientemente los recursos de los nodos del clúster.
- *Automatización*: Kubernetes automatiza muchas tareas relacionadas con la gestión de aplicaciones en contenedores.

### Elementos clave de Kubernetes

- *Nodos*: Un nodo es una máquina física o virtual que forma parte de un clúster de Kubernetes. Cada nodo ejecuta el software de Kubernetes y puede ejecutar uno o más pods.
- *Pods*: Un pod es la unidad básica de implementación en Kubernetes. Un pod puede contener uno o más contenedores que comparten recursos y se ejecutan en el mismo nodo.
- *Deployment*: Un deployment es un recurso de Kubernetes que se utiliza para gestionar la implementación de pods. Los deployments permiten desplegar y escalar aplicaciones de forma sencilla.
- *Servicios*: Un servicio es una abstracción que define un conjunto de pods y una política de acceso a ellos. Los servicios permiten a los pods comunicarse entre sí y con el mundo exterior.
- *Volúmenes*: Los volúmenes son unidades de almacenamiento que se pueden montar en los contenedores de un pod. Los volúmenes permiten a los contenedores compartir datos de forma persistente.
- *Ingress*: El Ingress es un recurso de Kubernetes que se utiliza para exponer servicios HTTP y HTTPS fuera del clúster. El Ingress permite enrutar el tráfico a los servicios en función de las reglas de configuración.


#### Pods

Un pod es la unidad básica de implementación en Kubernetes. Un pod puede contener uno o más contenedores que comparten recursos y se ejecutan en el mismo nodo. Los pods son efímeros y pueden ser eliminados y recreados en cualquier momento.

Para crear un pod en Kubernetes, es necesario definir un archivo de configuración en formato YAML que especifique las características del pod, como el nombre, las etiquetas, los contenedores y los volúmenes. A continuación se muestra un ejemplo de un archivo de configuración de un pod en Kubernetes:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
  - name: my-container
    image: nginx
    ports:
    - containerPort: 80
```

Para crear el pod en Kubernetes, se puede utilizar el comando `kubectl apply -f pod.yaml`, donde `pod.yaml` es el nombre del archivo de configuración del pod.

#### Deployments

Un deployment es un recurso de Kubernetes que se utiliza para gestionar la implementación de pods. Los deployments permiten desplegar y escalar aplicaciones de forma sencilla. Un deployment define el número de réplicas de un pod que se deben ejecutar y gestiona la actualización de la aplicación de forma automática.

Para crear un deployment en Kubernetes, es necesario definir un archivo de configuración en formato YAML que especifique las características del deployment, como el nombre, las etiquetas, los pods y las réplicas. A continuación se muestra un ejemplo de un archivo de configuración de un deployment en Kubernetes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:

  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        ports:
        - containerPort: 80
```

Para crear el deployment en Kubernetes, se puede utilizar el comando `kubectl apply -f deployment.yaml`, donde `deployment.yaml` es el nombre del archivo de configuración del deployment.

#### Servicios

Un servicio es una abstracción que define un conjunto de pods y una política de acceso a ellos. Los servicios permiten a los pods comunicarse entre sí y con el mundo exterior. Los servicios en Kubernetes pueden ser de varios tipos, como ClusterIP, NodePort y LoadBalancer.

Para crear un servicio en Kubernetes, es necesario definir un archivo de configuración en formato YAML que especifique las características del servicio, como el nombre, las etiquetas, los puertos y el tipo. A continuación se muestra un ejemplo de un archivo de configuración de un servicio en Kubernetes:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Para crear el servicio en Kubernetes, se puede utilizar el comando `kubectl apply -f service.yaml`, donde `service.yaml` es el nombre del archivo de configuración del servicio.

### ConfigMaps y Secrets

Los ConfigMaps y Secrets son recursos de Kubernetes que se utilizan para almacenar configuraciones y secretos de forma segura. Los ConfigMaps se utilizan para almacenar configuraciones de aplicaciones, como variables de entorno y archivos de configuración. Los Secrets se utilizan para almacenar secretos, como contraseñas y claves de API.

Para crear un ConfigMap en Kubernetes, se puede utilizar el comando `kubectl create configmap my-config --from-literal=key1=value1`, donde `my-config` es el nombre del ConfigMap y `key1=value1` es la configuración que se desea almacenar.

Para hacerlo con yaml:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
```

Para crear un Secret en Kubernetes, se puede utilizar el comando `kubectl create secret generic my-secret
--from-literal=key1=value1`, donde `my-secret` es el nombre

Para hacerlo con yaml:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:

  key1: dmFsdWUx
```

Para utilizar un ConfigMap o Secret en un pod, se puede montar el ConfigMap o Secret como un volumen en el pod y acceder a las configuraciones o secretos como archivos o variables de entorno.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    volumeMounts:
    - name: my-config
      mountPath: /etc/config
  volumes:
  - name: my-config
    configMap:
      name: my-config
``` 

[Volumenes](volumenes.md) 
