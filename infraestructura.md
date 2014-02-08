Gestión de infraestructuras virtuales
---------------------------------------------------------------------------------------------------------------------------
1.Instalar chef en la máquina virtual que vayamos a usar

```
apt-get install chef

```
2.Crear una receta para instalar nginx, tu editor favorito y algún directorio y fichero que uses de forma habitual.

1-Instalamos chef:
```
apt-get install chef
```
2-Despues creamos un fichero recetario.rb con la siguiente configuracion:
```
directory 'carpeta/'
    file "carpeta/receta" do
        owner "josef267"
        group "josef267"
        mode 00777
        action :create
        content "Crear receta"
        package 'nginx'
     end
```
3- ejecutamos la receta
```
chef-apply receta.rb
```
3.Escribir en YAML la siguiente estructura de datos en JSON:
```
{ uno: &quot;dos&quot;,tres: [ 4, 5, &quot;Seis&quot;, { siete: 8, nueve: [ 10, 11 ] } ] }
```
Para escribirlos en YAML:
```
---
- uno: "dos"
  tres:
    - 4
    - 5
    - "Seis"
    -
      - siete: 8
        nueve: 
          - 10
          - 11
```
4.Desplegar los fuentes de la aplicación de DAI o cualquier otra aplicación que se encuentre en un servidor git público en la máquina virtual Azure (o una máquina virtual local) usando ansible.

En mi caso voy a utilizar una maquina virtual de azure mas concretamente un ubuntu 12.04:

Lo primero que hare sera agregar el repositorio de ansible al ubuntu:
```
sudo add-apt-repository ppa:rquillo/ansible
```
Despues actualizaremos la lista de paqueres y instalaremos ansible:
```
sudo apt-get update
sudo apt-get install ansible
```
![captura 1](https://dl.dropbox.com/s/q5ikxhegs5l68th/ansible.png)

Nos dirigimos hasta la carpeta ansible(/etc/ansible/) y modificamos el fichero hosts:
```
[azure]
granadats.cloudapp.net
```
y comprobamos que todo esta correcto
![captura 3](https://dl.dropbox.com/s/91x38wmmabpjnm1/ansible3.png)

Apto seguido nos disponemos a instalar git para poder acceder y descargar las fuentes:
```
apt-get install git
```
![captura 2](https://dl.dropbox.com/s/s0vhs8lt0b32u3r/ansible1.png)

Ahora ya nos poremos descargar el proyecto directamente desde git:
```
ansible azure -m git -u josef267 --ask-pass -a "git@github.com:JoSeF267/Practica1-iv.git version=HEAD"
```

5.Desplegar la aplicación de DAI con todos los módulos necesarios usando un playbook de Ansible.

Usaremos el siguiente de configuracion para usar plaaybook de ansible:

```
---
- hosts: azure
  sudo: yes
  remote_user: josef267
  tasks:
    - name: Instalar Python y EasyInstall
      apt: name=build-essential state=present
      apt: name=python-dev state=present
      apt: name=python-setuptools state=present
    - name: Instalar MongoDB
      apt: name=mongodb-server state=present
    - name: Instalar módulos de Python necesarios
      command: easy_install web.py mako pymongo feedparser tweepy geopy
    - name: Crear servicio upstart
      template: src=dai.conf dest=/etc/init/dai.conf owner=root group=root mode=0644
    - name: Iniciar aplicación
      service: name=dai state=running
     ```
Ahora despues de realizar el fichero anterior solo nos quedaria ejecutarlo:

ansible-playbook dai-practica.yml
```
6.Instalar una máquina virtual Debian usando Vagrant y conectar con ella.

Primero instalaremos vagrant
```
sudo apt-get install vagrant
```
Ahora nos descargamos una maquina virtual ya configurada para vagrant:
```
vagrant box add debian https://dl.dropboxusercontent.com/u/197673519/debian-7.2.0.box
vagrant init debian

```
![captura 4](https://dl.dropbox.com/s/cukeod3ruolsi69/vagrant.png)

Ahora nos podemos levantamos la maquina y conectamos por ssh:
```
vagrant up
vagrant ssh

```
![captura 5](https://dl.dropbox.com/s/skui45ecfxrner9/vagrant1.png)

Y ya lo tenemos todo funcionando correctamente!! :

![captura 6](https://dl.dropbox.com/s/862zkjiz41t6h12/vagrant2.png)

7.Crear un script para provisionar nginx o cualquier otro servidor web que pueda ser útil para alguna otra práctica

Primero tendremos que modificar el fichero vagranfile:
```
config.vm.provision "shell",
    inline: "sudo apt-get update && sudo apt-get install -y nginx && sudo service nginx restart && sudo service nginx status"
```
y ahora le indicamos a vagrant que queremos provisionar esta maquina
```
vagrant provision
```
y ya lo tenemos todo listo!

8.Configurar tu máquina virtual usando vagrant con el provisionador ansible

Primero configuramos la ip de la maquina dentro del fichero Vagranfile
```
config.vm.network :private_network, ip: "192.168.3.50"
```

Ahora reiniciamos la maquina y la recargamos para aplicar los cambios:

```
vagrant reload
```
![captura 7](https://dl.dropbox.com/s/877u6v11xwoitdt/vagrant-ansible.png)

Ahora deberemos de indicarle a vagrant que queremos usar ansible como aprovisionamiento:

```
Vagrant.configure("2") do |config|
  config.vm.box = "debian"
  config.vm.network :private_network, ip: "192.168.3.50"

  config.vm.provision "ansible" do |ansible| 
    ansible.playbook = "provision.yml"
  end

end
```
Ahora mostrare el contenido de provision de configuracion:

```
---
- hosts: vagrant
  sudo: yes
  tasks:
    - name: Actualizar lista de paquetes
      apt: update_cache=yes
    - name: Instalar Apache
      apt: name=apache2 state=present
```
y simplemente deberemos de configurar el aprovisionamiento
```
vagrant provision
```
