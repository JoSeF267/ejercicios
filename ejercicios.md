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
