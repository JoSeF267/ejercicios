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

![captura 2](https://dl.dropbox.com/s/vf1row4ryidvowc/rubi-1.png)

3.¿Se pueden crear estructuras de datos mixtas en Ruby? Crear un array de hashes de arrays e imprimirlo.

Si se puede crear cualquier tipo de estructura dentro de un array.

```
$array={ :string=>[ 'ola','ke','ase'], :enteros['1','2','3']}
```

4.Crear una serie de funciones instanciadas con un URL que devuelvan algún tipo de información sobre el mismo: fecha de última modificación, por ejemplo. Pista: esa información está en la cabecera HTTP que devuelve

Aqui tenemos el codigo:
```
#!/usr/bin/ruby

    require 'net/http'

    def ultima_fecha()
        response = Net::HTTP.get_response('www.shippuden.tv','/')     
        return response['date'].to_s
    end

    def ultima_conexion()
        response = Net::HTTP.get_response('www.shippuden.tv','/')     
        return response['content-type'].to_s
    end

    def informacion_servidor()
        response = Net::HTTP.get_response('www.shippuden.tv','/')     
        return response['server'].to_s
    end

    puts ('ultima fecha:') 
    puts ultima_fecha()

    puts ('contenido servidor')   
    puts ultima_conexion()

    puts ('tipo de servidor:') 
    puts informacion_servidor()

```
Aqui tenemos la ejecucion del programa:

![captura 3](https://dl.dropbox.com/s/hxwtdq1hnr73jhf/servidor.png)

5.Ver si está disponible Vagrant como una gema de Ruby e instalarla.

para ver si esta disponible vagrant utilizamos la siguiente orden:
```
gem search --remote vagrant
```
y despues para instalarlo utilizamos:

```
sudo gem install vagrant
```
