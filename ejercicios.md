Ejercicios 11112013
--------------------------------------------------------------------------------------------

1. Instala LXC en tu versión de Linux favorita.

```
apt-get install lxc
```

2. Comprobar qué interfaces puente ha creado y explicarlos

![captura 1] (https://dl.dropbox.com/s/xq47zr01xwc22u2/puente.png)


3.1.Crear y ejecutar un contenedor basado en Debian
```
sudo lxc-create -t ubuntu -n una-caja
```
3.2.Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya.

```
error con fedora!!!
```
4.1 Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.
```
wget http://lxc-webpanel.github.io/tools/install.sh -O - | bash

```
![captura 2] (https://dl.dropbox.com/s/014842k2d3jlgr3/lxc-panel.png)

4.2 Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.
![captura 3] (https://dl.dropbox.com/s/vf2694uliis8ilp/recursos.png)

5 Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.

Dentro del contenedor installamos ab:
```
apt-get install apache2-utils
```
y lo probamos de esta forma

```
 ab -n 50 -c 5 http://localhost/
```

6.1 Instalar juju

Para instalar juju, vamos a realizar lo siguiente:
```
 sudo add-apt-repository ppa:juju/stable
```
Y actualizamos:
```
sudo apt-get update
```
E instalamos el siguiente paquete:
```
sudo apt-get install juju-core
```
Llegados a este punto, instalamos MongoDB mediante:
```
sudo apt-get install mongodb-server
```
Ahora creamos un tapper de la siguiente forma:
```
sudo juju bootstrap
```
y creamos el archivo de configuración de juju con:
```
juju init
```
Y editamos ~/.juju/environments.yaml donde default: amazon lo cambiamos por:
```
default: local
```
Cambiamos el tipo de tapper con: sudo juju switch local. E instalamos mysql para integrar mediawiki:
```
sudo juju deploy mediawiki
sudo juju deploy mysql
```
Y los combinamos:
```
  sudo juju add-relation mediawiki:db mysql
  juju expose mediawiki 
```
![captura1](https://dl.dropbox.com/s/280hchbbeizkrq2/juju.png)


7.1 Destruir toda la configuración creada anteriormente

Para destruir toda la configuraicon:
```
sudo juju remove-unit mediawiki/0 mysql/0
```
Y para borrar las maquinas usariamos :
```
sudo juju destroy-machine 2
```

7.2 Volver a crear la máquina anterior y añadirle mediawiki y una relación entre ellos.
creamos el archivo de configuración de juju con:
```
juju init
```
Y editamos ~/.juju/environments.yaml donde default: amazon lo cambiamos por:
```
default: local
```
Cambiamos el tipo de tapper con: sudo juju switch local. E instalamos mysql para integrar mediawiki:
```
sudo juju deploy mediawiki
sudo juju deploy mysql
```
Y los combinamos:
```
  sudo juju add-relation mediawiki:db mysql
  juju expose mediawiki 
```
![captura1](https://dl.dropbox.com/s/280hchbbeizkrq2/juju.png)


7.3 Crear un script en shell para reproducir la configuración usada en las máquinas que hagan falta.

