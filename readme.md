# 3er Meetup de Golang

![Gopher Mty](mty_gopher.jpg)


- [Instalación de Go](#instalación-de-go)
- [Instalación de Beego](#instalación-de-beego)
- [Creación de la App](#creación-de-la-app)
- [Referencias](#referencias)

## Instalación de Go

Puedes instalar Golang de dos formas:

1. De forma manual siguiendo las instrucciones de la página [oficial](https://golang.org/doc/install).  
2. A través de [Gvm](https://github.com/moovweb/gvm): Go Version Manager

*Es recomendable tener Go 1.4 o mayor*

```bash
$ go version
go version go1.5.1 darwin/amd64
```

## Instalación de Beego

Para poder usar Beego, hay que instalarlo junto con Bee (*command line utilities*). [Aquí](http://beego.me/quickstart) están las instrucciones completas. Básicamente es:

```bash
$ go get github.com/astaxie/beego
$ go get github.com/beego/bee
```

Y después para verificar que fue instalado correctamente: 

```bash
$ bee version
bee   :1.4.1
beego :1.6.0
Go    :go version go1.5.1 darwin/amd64
```

## Creación de la app



### Base de datos

Necesitamos una base de datos cualquiera (MySql o PostgreSQL). También puedes ocupar una de [ejemplo](https://dev.mysql.com/doc/employee/en/employees-installation.html)


### Creación de la API

Con una base de datos en mysql

```bash
$ bee api funky -driver="mysql" -conn="dummy:secret123@tcp(127.0.0.1:3306)/funky_development"
```

- `dummy`: El nombre del usuario
- `secret123`: Password
- `funky_development`: Nombre de la base de datos

Con una base de datos en postgres

```bash
bee api funky -driver="postgres" -conn="postgres://dummy:secret123@127.0.0.1:5432)/funky_development
```

- `dummy`: El nombre del usuario
- `secret123`: Password
- `funky_development`: Nombre de la base de datos


Temporalmente deshabilita la documentacion:

```golang
// conf/app.conf
appname = funky
httpport = 8080
runmode = dev
autorender = false
copyrequestbody = true
EnableDocs = false
```

```
// main.go
package main

import (
	// _ "funky/docs"
	_ "funky/routers"
	"github.com/astaxie/beego"
	"github.com/astaxie/beego/orm"
	_ "github.com/go-sql-driver/mysql"
)
....
```

Para iniciar el servidor:

```bash
$ cd funky
$ bee run
```

![](beego_screenshot.png)

Puedes hacer cosas como:

```
http://127.0.0.1:8080/v1/cats/?sortby=name&order=desc

http://127.0.0.1:8080/v1/cats/?limit=100

http://127.0.0.1:8080/v1/cats/?sortby=name&order=desc&limit=100
```

o desde curl

```bash
curl -X POST -H "Content-Type: application/json" -d '{"Age": 10, "Name": "Grumpyy", "CreatedAt": "2016-03-03T22:12:17-06:00", "UpdatedAt": "2016-03-03T22:12:17-06:00"}' http://127.0.0.1:8080/v1/cats/
```

Para detener el servidor presiona con `Ctrl + c`


### Documentación 

Beego tiene un panel administrativo y generación automática incluida a través de [swagger](http://swagger.io). Para habilitarlos, hay que activar las opciones de documentación automática:

```golang
// conf/app.go
appname = funky
httpport = 8080
runmode = dev
autorender = false
copyrequestbody = true
EnableDocs = true
EnableAdmin = true
```

```golang
// main.go
package main

import (
	_ "funky/docs"
	_ "funky/routers"
	"github.com/astaxie/beego"
	"github.com/astaxie/beego/orm"
	_ "github.com/go-sql-driver/mysql"
)
....
```

```bash
bee run -downdoc=true -gendoc=true
```

Admin dashboard por default corre en `localhost:8088`

![](beego-admin-dashboard.png)

Swagger documentation en `localhost:8080/swagger/swagger-1/`

![](swagger-ui.png)


## Referencias

- [Golang Mty](http://www.meetup.com/Golang-MTY/events/229070594/)
- [@may-cabrera](https://twitter.com/may_cabrera), [@arnoldocolin](https://twitter.com/arnoldocolin)
- [Proyecto de Rails](https://github.com/mayra-cabrera/funky)
- [Funky API Golang](https://github.com/mayra-cabrera/funky_golang)
