Introducción ligera al lenguaje Ruby
----------------------------------------------------------

1.Instalar Ruby y usar

Instalamos ruby:
```
sudo apt-get install ruby
```
para comprobar la versión instalada. A la vez, conviene instalar también irb, rubygems y rdoc.

```
ruby --version
```
y comprobamos la version instalada:
![captura 1](https://dl.dropbox.com/s/1pre8mwtde3tdbt/ruby.png)

2.Crear un programa en Ruby que imprima los números desde el 1 hasta otro contenido en una variable.

![captura 2](https://www.dropbox.com/s/vf1row4ryidvowc/rubi-1.png)

3.¿Se pueden crear estructuras de datos mixtas en Ruby? Crear un array de hashes de arrays e imprimirlo.

Si se puede crear cualquier tipo de estructura dentro de un array.

```
$array={ :string=>[ 'ola','ke','ase'], :enteros['1','2','3']}
```
