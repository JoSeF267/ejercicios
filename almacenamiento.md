Virtualización del almacenamiento
-------------------------------------------------------------------

1.1¿Cómo tienes instalado tu disco duro? ¿Usas particiones? ¿Volúmenes lógicos?

![captura 1](https://dl.dropbox.com/s/wm3ywdirgowo4a4/disco.png)


2.Usar FUSE para acceder a recursos remotos como si fueran ficheros locales. Por ejemplo, sshfs para acceder a ficheros de una máquina virtual invitada o de la invitada al anfitrión.



3.Crear imágenes con estos formatos (y otros que se encuentren tales como VMDK) y manipularlas a base de montarlas o con cualquier otra utilidad que se encuentre

Para el sistema raw lo haremos de la siguiente forma:
```
dd of=imagen.img bs=1k
```
Para el sistema qcow2 de la siguiente forma:
```
qemu-img create -f qcow2 imagen.qcow2 5M
```
y para VMDK de la siguiente forma:
```
qemu-img create -f vmdk imagen.vmdk 5M
```

4.Crear uno o varios sistema de ficheros en bucle usando un formato que no sea habitual (xfs o btrfs) y comparar las prestaciones de entrada/salida entre sí y entre ellos y el sistema de ficheros en el que se encuentra, para comprobar el overhead que se añade mediante este sistema

Primero creamos las imagenes con qmenu
```
qemu-img create -f raw ej4-xfs.img 512M
```
Lo convertimos a ficheros en bucle para poder asociarlo a archivos con bucle de archivos regulares.

```
sudo losetup -v -f ej4-xfs.img
```
Lo formateamos:
```
sudo mkfs.xfs /dev/loop1
```
y por ultimo lo montamos para terminar y comprobar su funcionamiento:

```
sudo mount /dev/loop0 /mnt/loop1/
```

![captura 2](https://dl.dropbox.com/s/gd4f7n37jravbyn/loop.png)

5.Instalar ceph en tu sistema operativo.
```
sudo apt-get install ceph-mds
```
![captura 3](https://dl.dropbox.com/s/jzzcsxqa678ztod/ceph.png)

6.Crear un dispositivo ceph usando BTRFS o XFS

Creamos la imagen y le damos formato:
```
qemu-img create -f raw imagen.img 512M
sudo mkfs.xfs /dev/loop2
```
Arrancamos ceph:
```
/etc/init.d/ceph -a start
```
y montamos la imagen:
```
sudo mount -t ceph ubuntu:/ /mnt/ceph
```

7.Almacenar objetos y ver la forma de almacenar directorios completos usando ceph y rados.
Primero creamos una piscina:
```
sudo rados mkpool piscina-ejercicio7
```
Despues añadimos el objeto que queremos:
```
sudo rados put -p piscina-ejercicio7 objeto ejercicio7.png
```
Y lo listamos, para comprobar que se ha creado:
```
rados ls -p prueba-piscina
```
8.Tras crear la cuenta de Azure, instalar las herramientas de línea de órdenes (Command line interface, cli) del mismo y configurarlas con la cuenta Azure correspondiente.

Primero instalamos nodejs:
```
apt-get install nodejs
```
Ahora instalamos la plataforma de azure
```
npm install azure-cli
```
![captura 4](https://dl.dropbox.com/s/c2h11p7tn79r9hl/npm.png)

Entramos en la siguiente direccion y descargamos el fichero e importamos:
```
http://go.microsoft.com/fwlink/?LinkId=254432
azure account import Azpad245WBV9818-1-12-2014-credentials.publishsettings
```
![captura 5](https://dl.dropbox.com/s/gtaf23spks0mh4y/azure.png)

Ahora creamos la cuenta de almacenamiento:

```
azure account storage create josef267
azure account storage keys list josef267
AZURE_STORAGE_ACCOUNT=josef267
export AZURE_STORAGE_ACCOUNT=llaveprimaria
```
y ya lo tendriamos todo configurado para trabajar con azure.

9.Crear varios contenedores en la cuenta usando la línea de órdenes para ficheros de diferente tipo y almacenar en ellos las imágenes en las que capturéis las pantallas donde se muestre lo que habéis hecho.

Primero creamos el contenedor:
```
azure storage container create capturas -p blob
```
![captura 6](https://dl.dropbox.com/s/8h6e88yp36l3mlz/azure1.png)

y ahora subimos la imagen:
```
azure storage blob upload azure1.png
```
y ya tenemos la captura subida a nuestra cuenta azure:

![captura 7](https://dl.dropbox.com/s/7nad8x5jfh0rpmb/subiendo-azure.png)

10.Desde un programa en Ruby o en algún otro lenguaje, listar los blobs que hay en un contenedor, crear un fichero con la lista de los mismos y subirla al propio contenedor. Muy meta todo.

Instalamos la gem de azure:
```
gem install azure
```
y ejeecutamos el siguiente codigo:

```
#!/usr/bin/ruby
require "azure"

azure_blob_service = Azure::BlobService.new
containers = azure_blob_service.list_containers()

containers.each do |container|
        name = container.name + ".txt"

    File.open(name, "w") do |list|

            list.puts container.name + ":"
            list.puts "=" * container.name.length

        blobs = azure_blob_service.list_blobs(container.name)

            blobs.each do |blob|
                    list.puts "\t" + blob.name
            end
        end

        content = File.open(name, "rb") { |file| file.read }
        blob = azure_blob_service.create_block_blob(container.name, name, content)

end


```
