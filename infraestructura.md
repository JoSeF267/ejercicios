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
ansible azure -m git -u josef267 --ass-pass -a "git@github.com:JoSeF267/Practica1-iv.git" 
```

