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
