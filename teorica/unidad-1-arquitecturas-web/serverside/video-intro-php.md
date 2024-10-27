hay una dieferencia entre lo que es un servidor y un servidor web. Un servidor es un nodo fisico dónde estan instaladas todas nuestras aplicaciones dea backend. Un servidor web es un software instalado dentro de este nodo el cual es el encargado de gestionar las solicitudes del cliente y despachr el contendio solicitado. Es importante hacer esta diferenciacion porque si bien a veces nos referimos a uno y otro de la misma manera, es el servidor web el que convive dentro del servidor fisico. 

En este video vamos a generar una aplicacion web en el servido, usando XAMPP para hacer de nuestra pc un servidor y posicionando nuestras carpetas y archivos en la carpeta htdocs para que tenga acceso a ellos.

En el servidor, un archivo php puede además de ser php, actuar como un archivo html, ya que el srvidor puede interpretarlo y de esta manera poder generar contenido dinamico. Es decir, vamos a tener un archivo `index.php`, que el servidor va a interpretar y mostrar un html al momento de "servir" la página. Este archivo, que mezcla ambos lenguajes, se ve de la siguiente manera: 

```php
<!--No vamos a escribir el head por cuestiones de sólo ejemplizar-->
<body>
    <h1>Esto es contenido dinamico</h1>
    <p>El servidor lo interpreta y el navegador lo muestra como HTML</p>

    <?php
    $var = "Esto es contenido dinamico, el servidor ejcuta este código y luego lo devuelve como html para que lo muestre el navegador como tal"; 

    echo "<p>$var</p>"
    >

</body>
```
```php
    <?php

        $arregloNombres = array("Agus", "Fernando", "Fermin")


        foreach($names as $name){
            echo $name;//esto así nomás va a concatener los nombres quedando algo como agusfernandofermin

        }
        echo "<ul>"
        foreach($names as $name){
            echo "<li>" . $name . "</li>";

        }
        echo "</ul>";
    >
```
```php
```