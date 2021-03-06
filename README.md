[![Go Report Card](https://goreportcard.com/badge/gojp/goreportcard)](https://goreportcard.com/report/github.com/theboshy/ClientConsum) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/gojp/goreportcard/blob/master/LICENSE) <a href="https://github.com/theboshy/ClientConsum/stargazers">
    <img src="https://img.shields.io/github/stars/theboshy/ClientConsum.svg?style=social" alt="GitHub stars">
  </a>
  
  [DOCKERHUB](https://hub.docker.com/r/devile/clientconsum/)


# Client Consum API <img style="display:inline-block" width="40" heigth="40" src="https://user-images.githubusercontent.com/14255055/38960106-d2fb221e-4328-11e8-85b7-ca809bf39918.png">
Api client para KuberProject con servicios rest implementando el framework gin-gonic

Este servicio se encarga de consumir a <a href="https://github.com/theboshy/KuberProject"> **KuberProject** </a> <img style="display:inline-block" width="40" heigth="40" src="https://png.icons8.com/ios/50/000000/developer.png">, por medio de conexion **rpc**

Despues de resolver la solicitud por **API REST** , se conectara mediante el protocolo **buf** , a el servidor **tcp** 
en *kuberproject*

### Requerimientos 
* [Minikube](https://github.com/kubernetes/minikube) - (mini) servicio local de *kubernetes* 
* [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) - herramienta de línea de **comandos** *(cli)* de *Kubernetes*
* [ProtoBufCompiler](https://github.com/google/protobuf) - compilador de **proto buf**
* [GoProtoBufCompiler](https://github.com/golang/protobuf) - compilador de proto buf para **golang**
* [VirtualBox](https://www.virtualbox.org/) - creador de **maquinas virtuales** para *win*


### Build

```sh
$ cd ./[<project_path>]

# cosntruir protobuf *pb*
$ protoc -I ./mcs --go_out=plugins=grpc:./pb ./mcs/*.proto

#minikube mantiene un servicio docker el cual podemos usar para generar nuestro contenedor e imagen
$ eval $(minikube docker-env)
#generar la imagen con el servicio ClientConsum
$ docker build -t [<docker_image_name>] -f Dockerfile.api .

#generar el nodo contenedor del servicio cconsum 
$ kubectl apply -f api-deployment.yaml

```
### Archivos descriptores
[deployment.yaml](https://github.com/theboshy/ClientConsum/blob/master/api-deployment.yaml)

[Dockerfile.api](https://github.com/theboshy/ClientConsum/blob/master/Dockerfile.api)


### Test
Para comunicarce con el *api* es necesario conocer su ubicacion dentro del `minikube cluster`
```sh
$ minikube service api-service --url
http://xxx.xxx.xx.xx:xxxx
```

```sh
$ curl http://xxx.xxx.xx.xx:xxxx/gcd/6/2
```


-----

> support **[net/http/pprof]**

### Instalar pprof
[net/http/pprof](https://golang.org/pkg/net/http/pprof/)
```sh
$ go get github.com/DeanThompson/ginpprof
```

### Profiler End-routers
``` go
GET("/debug/pprof/")
GET("/debug/pprof/heap")
GET("/debug/pprof/goroutine")
GET("/debug/pprof/block")
GET("/debug/pprof/threadcreate")
GET("/debug/pprof/cmdline")
GET("/debug/pprof/profile")
GET("/debug/pprof/symbol")
POST("/debug/pprof/symbol")
GET("/debug/pprof/trace")
GET("/debug/pprof/mutex")
```
### Uso de profiler
> Nota : este ejemplo se muestra fuera del `cluster` de `minikube` localmente

```sh
$  go tool pprof goprofex http://localhost:3000/profiler/debug/pprof/profile/
$  go tool pprof goprofex http://localhost:3000/profiler/debug/pprof/heap/
```

### Generar Graficas con Graphviz2.38
Descargar
[Graphviz](https://graphviz.gitlab.io/download/)

Descargar e instalar con [python](https://www.python.org/) 
<img  width="35" heigth="35" src="http://www.pngall.com/wp-content/uploads/2016/05/Python-Logo-Free-Download-PNG.png">
```sh
$ pip install graphviz
```
Descargar e instalar con [chocolatey](https://chocolatey.org/) 
<img width="35" heigth="35" src="https://user-images.githubusercontent.com/14255055/38960311-a713a5f8-4329-11e8-9d01-aeb43bc1d511.png">

```sh
$ choco install graphviz
```


### Instalar 
Crear variable de entorno para **graphviz** en *path*

![captura](https://user-images.githubusercontent.com/14255055/38958417-cf0b53e6-4322-11e8-993b-df7850a63518.PNG)

### Usar Profiler con Graphviz
```sh
$ go tool pprof goprofex http://xxx.xxx.xx.xx:xxxx/profiler/debug/pprof/profile/
..... Entering interactive mode 
$ (pprof) web

```

![captura](https://user-images.githubusercontent.com/14255055/38959396-26b2e0ac-4326-11e8-9ac0-d1827aed1357.PNG)
