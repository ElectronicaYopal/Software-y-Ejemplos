# Software y configuración
Instalación de software y ejemplos de prueba

Nota: Información presentada para trabajar sobre Debian/Ubuntu

Tener presente antes de:
```bash
sudo apt update
sudo apt upgrade
```

## Instalación git
1. Istalar git
```bash
sudo apt install git
```
2. Comprobar instalación
```bash
git --version
```
3. Configurar cuenta
```bash
git config --global user.name "Usuario"
git config --global user.email "correo@dominio.com"
```
4. Comprobar configuración
```bash
git config --list
```
### Clonar repositorio localmente
1. Clonar el repositorio a trabajar
```bash
git clone https://github.com/Usuario/Repositorio
```
2. Ubicarse dentro del repositorio
```bash
cd Repositorio
```
### Modificar y sincronizar con Github
Trabajar en el directorio del repositorio y para cargar los cambios a Github:
1. Hacer commit/comentario de los cambios hechos
```bash
git commit -m "comentario especificando cambios"
```
2. Cargar los cambios a Github
```bash
git push
```
Para sincronizar cambios remotos del repositorio:
1. Hacer
```bash
git pull
```
Nota: Ser muy cuidados@ con lo que modifica o borra, es preferible crear una branch y no tocar el repo principal.

## Instalación Golang
1. Descargar la última versión estable de: https://golang.org/dl/ 
2. Ubicarse en la carpeta donde se ha descargado el go1*.tar.gz y descomprimir con:
```bash
sudo tar -C /usr/local -xzf go1*.tar.gz
```
3. Configurar variables de entorno ingresando al siguienge archivo:
```bash
sudo nano /etc/profile.d/goenv.sh
```
Agregar lo siguiente en goenv.sh
```bash
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```
Ctrl+O, Ctrl+X para guardar y salir.
4. Actualizar las variables de entorno
```bash
source /etc/profile.d/goenv.sh
```
5. Crear el directorio de trabajo en Home
```bash
mkdir -p  ~/go
```
6. Comprobar instalación
```bash
go version
echo $GOPATH
```

### Variables para manejo de GO111MODULE
```bash
# Comando para consultar variable del sistema:  
env

# Definir variables
export GO111MODULE=on

# Eliminiar variables
unset GO111MODULE
```
```bash
# Comando para consultar variable de golang:
 go env

# Definir variables
go env -w GO111MODULE=on

# Eliminiar variables
go env -w GO111MODULE=""
```

## Instalación de bee
1. Instalar bee
```bash
go get github.com/beego/bee/v2
```
2. Comprobar instalación
```bash
bee version
```

## Instalación de postgresql
1. Istalar postgresql y paquetes adicionales necesarios
```bash
sudo apt install postgresql postgresql-contrib
```
2. Comprobar instalación
```bash
psql -V
```
### Establecer contraseña para el usuario por defecto: postgres
1. Ingresar al prompt de postgresql
```bash
sudo -u postgres psql
```
2. Estando dentro de postgres, asigne la contraseña de la siguiente manera:
```bash
postgres=# ALTER USER postgres PASSWORD 'contraseña';
```
3. Para salir del prompt de postgres digite: \q o exit

## Instalar pgAdmin4
1. Instalar pgAmin4 con entorno desktop y web
```bash
sudo apt install pgadmin4
```
Una vez instalado debe aparecer el ícono de la aplicación.
2. Configurar entorno web
```bash
sudo /usr/pgadmin4/bin/setup-web.sh
```
Le pedirá crear un usuario o correo x y contraseña, para acceder: http://localhost/pgadmin4

Eso es todo por el momento.
