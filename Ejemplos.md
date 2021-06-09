# Ejemplo API CRUD
Ejemplo de API CRUD con Bee y PostgreSQL

## Crear base de datos y tabla con pgAdmin4
Ingresando mediante aplicación o interfaz web a pgAdmin4 implementar una base de datos como esta:

![Ejemplo db,tabla postgres](/pictures/pgadmin4_ejem1.png)

## Crear api con Bee
1. Dirigirse al directorio go/src
```bash
cd $GOPATH/src
```
2. Crear la api vinculándola con la base de datos creada
```bash
bee api nombre_api_crud -driver=postgres -conn="postgres://postgres:contraseña_postgres@127.0.0.1/nombre_basedatos?sslmode=disable"
```
Si la apliación ha sido satisfactoriamente creada, se debe haber creado un directorio como el siguiente:

![directorio de api](/pictures/directorioapicrud.png)

3. Ubicarse dentro del directorio de la api y descargar las dependencias que hagan falta
```bash
cd nombre_api_crud
go mod tidy
```
4. Correr la aplicación
```bash
bee run
```
5. En caso de presentar error con `multiple-value web.AppConfig.Strings() in single-value context` editar el main.go
```bash
sudo nano main.go
```
Original:
```bash
....
func main() {
	orm.RegisterDataBase("default", "postgres", beego.AppConfig.String("sqlconn"))
	if beego.BConfig.RunMode == "dev" {
....
```
Modificación:
```bash
....
func main() {
	sqlconn, _ := beego.AppConfig.String("sqlconn")
	orm.RegisterDataBase("default", "postgres", sqlconn)
	if beego.BConfig.RunMode == "dev" {
....
```
6. Ejecutar y generar documentación y swagger
```bash
bee run -downdoc=true -gendoc=true
```
En este momento la API ya debe ser accesible desde el navegador mediante http://localhost:8080/swagger
![Api running swagger view](/pictures/apicrudswagger.png)

En la terminar se monitorean los request hechos a la api
![Terminal monitoreo request](/pictures/beerunningapi.png)

Y el directorio de la api termina asi:
![Estado final directorio api](/pictures/directorioapifinal.png)

La api ya se puede ejecutar sin recurrir a bee run, simplemente digitando en nombre de esta.

Para hacerla accesible localmente se habilita el puerto en el firewall
```bash
sudo ufw allow 8080
```

Para exponerla en línea se usa ngrok, [más info aquí.](README.md#instalar-ngrok)

NeFa
