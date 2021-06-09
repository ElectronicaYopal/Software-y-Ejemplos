# Software y Ejemplos
Instalación de software y ejemplos de prueba

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

## Instalación bee
