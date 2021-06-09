# Software y configuración
Instalación de software y ejemplos de prueba

Nota: Información presentada para trabajar sobre Debian/Ubuntu

Tener presente antes de:
```bash
sudo apt update
sudo apt upgrade
```
Software:
- [Git](#instalación-git)
- [Golang](#instalación-golang)
- [Bee](#instalación-de-bee)
- [Postgresql](#instalación-de-postgresql)
- [pgAdmin4](#instalar-pgadmin4)
- [pgModeler](#instalar-pgmodeler)
- [Ngrok](#instalar-ngrok)
- [SSH](#instalar-ssh)
- [xRDP](#instalar-xrdp)
- [Samba](#instalar-samba)


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

## Instalación de Bee
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

### Configuración para ser accesible desde pgModeler
1. Configurar autenticación de postgresql en el archivo pg_hba.conf
```bash
sudo nano /etc/postgresql/Numero_Version_Postgres/main/pg_hba.conf
```
2. Cambiar METHOD a "trust" lo siguiente señalado con la flecha:
```bash
....
# Database administrative login by Unix domain socket
local   all             postgres                                trust <--- antes: peer

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust <--- antes: peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust <--- antes: md5
# IPv6 local connections:
host    all             all             ::1/128                 trust <--- antes: md5
# Allow replication connections from localhost, by a user with the
# replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
....
```
3. Reiniciar postgres
```bash
sudo service postgresql restart
```

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

## Instalar pgModeler
1. Instalar pgModeler
```bash
sudo apt-get install pgmodeler
```
2. Crear acceso directo
```bash
sudo nano /usr/share/applications/pgmodeler.desktop
```
Dentro del archivo agregar:
```bash
[Desktop Entry]
Name=pgModeler
Type=Application
Exec=pgmodeler
Terminal=false
Icon=/usr/local/share/pgmodeler/conf/pgmodeler_logo.png
Comment=Integrated Development Environment
NoDisplay=false
Categories=Development;IDE;
Name[en]=pgModeler
```
Ctrl+O para guardar y Ctrl+X para salir

## Instalar NGROK
1. Registrarse en https://ngrok.com/
2. Instalar NGROK desde snap, Nota: snap se encuentra instalado por defecto desde Ubuntu 16.04, sino seguir los pasos en página de NGROK[https://ngrok.com/download]
```bash
sudo snap install ngrok
```
3. Al registrase se genera un token, este se ingresa a la aplicación Ngrok
```bash
ngrok authtoken <your_auth_token>
```
4. Con los siguientes comandos puede exponer a la web distintos servicios
```bash
# SSH
ngrok tcp 22

# xRDP
ngrok tcp 3389

# HTTP ejemplo para puerto 80
ngrok http 80

# HTTP Evitar error: Invalid Host Header, ejemplo puerto 8080 
ngrok http --host-header=rewrite 8080
```
Nota: con cuenta gratuita solo puede ejecutar una sesión al tiempo.

## Instalar SSH
1. Instar SSH
```bash
sudo apt install openssh-server
```
2. Verificar que el servicio se está ejecutando
```bash
sudo systemctl status ssh
```
Para salir presione Q.

3. Si el servicio no se está ejecutando, hacer:
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```
4. Permitir ssh en firewall y verificar
```bash
sudo ufw allow ssh
sudo ufw enable

sudo ufw status
# debe aparecer el puerto tcp/22 para ssh
```
Ahora es accesible remotamente de manera local y en linea ejecutando ngrok.

## Instalar xRDP
1. Instalar xrdp
```bash
sudo apt install xrdp
```
2. Verificar que el servicio se está ejecutando
```bash
sudo systemctl status xrdp
```
3. Si no se está ejecutando
```bash
sudo systemctl restart xrdp
```
4. Añadir certificado SSL a usuario
```bash
sudo adduser xrdp ssl-cert
```
5. Permitir xrdp en firewall
```bash
sudo ufw allow 3389
sudo ufw reload

sudo ufw status
# debe aparecer el puerto tcp/3389
```

## Instalar Samba
1. Instalar samba
```bash
sudo apt install samba
```
2. Verificar que el servicio está activo
```bash
sudo systemctl status smbd
sudo systemctl status nmbd
```
3. Si no está activo, entonces:
```bash
sudo systemctl start smbd
sudo systemctl start nmbd
```
4. Permitir samba en firewall
```bash
sudo ufw allow samba
```
5. Añadir usuario de acceso al directorio a compartir
```bash
sudo adduser nombre_usuario
```
6. Establecer contraseña
```bash
sudo smbpasswd -a nombre_usuario
```
7. Crear grupo para el usuario
```bash
sudo groupadd nombre_grupo
```
8. Añadir el usuario al grupo
```bash
sudo gpasswd -a nombre_usuario nombre_grupo
```
9. Crear si no existe el directorio a compartir
```bash
mkdir /ruta/del/directorio
```
10. Otorgar permisos de lectura y escritura al directorio a compartir
```bash
sudo setfacl -R -m "g:nombre_grupo:rwx" /ruta/del/directorio
```
11. Editar el archivo de configuración de samba
```bash
sudo nano /etc/samba/smb.conf
```
12. Verificar/cambiar en nombre del grupo de trabajo en:
```bash
[global]

## Browsing/Identification ###

# Change this to the workgroup/NT-domain name your Samba server will part of
   workgroup = WORKGROUP
....
```
Por defecto para compartir con Windows el grupo de trabajo es WORKGROUP.

13. Al final del archivo smb.conf, añadir:
```bash
.....
[Sharing]
   path = /ruta/del/directorio
   valid users = nombre_usuario
   guest ok = yes
   browsable = yes
   writeable = yes
   read only = no
   force user = nombre_usuario
   force create mode = 0777
   force directory mode = 0777
```
Crtl+O para guardar y Ctrl+X para cerrar.

14. Reiniciar el servicio de samba
```bash
sudo systemctl restart smbd nmbd
```

## Password Root - Usar bajo su responsabilidad !!!
```bash
# Set Pass
sudo passwd root

# Delet Pass
sudo passwd -dl root
```
Eso es todo por ahora... NeFa!
