Virtualización completa: uso de máquinas virtuales
==========================================================================================================================

1.Instalar los paquetes necesarios para usar KVM. Se pueden seguir estas instrucciones. Ya lo hicimos en el primer tema, pero volver a comprobar si nuestro sistema está preparado para ejecutarlo o hay que conformarse con la paravirtualización.

Primero instalamos el paquete , tendria que tenerlo instalado de antes pero mi ubuntu lo borro:

```
sudo apt-get install qemu-kvm libvirt-bin
```

y despues realizamos el comando correspondiente y todo ok!!

![captura 1](https://dl.dropbox.com/s/hislbyoop3847oz/kvm.png)

2.1.Crear varias máquinas virtuales con algún sistema operativo libre, Linux o BSD. Si se quieren distribuciones que ocupen poco espacio con el objetivo principalmente de hacer pruebas se puede usar CoreOS (que sirve como soporte para Docker) GALPon Minino, hecha en Galicia para el mundo, Damn Small Linux, SliTaz (que cabe en 35 megas) y ttylinux (basado en línea de órdenes solo).

Primeros nos bajhttps://dl.dropbox.com/s/obj7y9igdb9o8h6/coreos.png)

Ahora instalaremos DAMN SMALL LINUX, lo primero que haremos sera descargar la imagen:

```
wget ftp://distro.ibiblio.org/pub/linux/distributions/damnsmall/current/dsl-4.4.10.iso
```

amos la imagen de coreOS y el script de instalacion que amablemente nos dejan en la pagina web:
```
wget http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_qemu_image.img.bz2
wget http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_qemu.sh
```

Despues descomprimimos e instalamos con el scrtip y listo ya tenemos funcionando el coreOS:

![captura 2](https://dl.dropbox.com/s/obj7y9igdb9o8h6/coreos.png)
