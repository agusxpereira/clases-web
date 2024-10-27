php es debilmente tipado. Aunque sus versiones más nuevas pwemiten realizar casteos. Ya que al ser un lenguaje de backend, estos permiten menos errores.

Las paginas se generan de manera dinamica, no hay un HTML por cada noticia por ejemplo.  Se contruyen en vase archivos en base de datos y consultas a base de datos.

POr ahora podemos mezclar php y hmtl, pero mñas adelante vamos a serparlos. (siempres en archivos .php) Al cleinte llega un HTML, lo que está entre el bloque <?php se ejecuta y se interpreta. 

extensiones: php inteliphense

arreglo indexado: arr[0][1][2]....


Declarar un arreglo en php: 
```php
$arreglo = ['Agus', 'Nose', 'Hola'];
//arreglo asociativo

foreach($nombres as $nombre){
    echo "<li> $nombre (edad: $edades[$nombre])* </li>"
}
echo "Hola $arreglo[0]";

//arreglo asociativo
$edades = [
    'Agus' => '24', 
    'Nose' => '111',
    'Hola' => '111'
]
echo $edades['Agus'];//24
//también podemos recorrelos o hacer cosas más locas como en el ejemplo de arriba*: 


```
A un arreglo asociativo podemos pedirle la clave igualmente, lo que hace que el primer arreglo digamos no sea util en el ejemplo


```php
$edades = [
    'Agus' => '24', 
    'Nose' => '111',
    'Hola' => '111'
]
foreach($edades as $key => $value){
    "<li>$key: $value</li>";
}

//imprime lo mismo
```

Básicamente nos sirve para hacer maps, algunas base de datos nos devuelve este tipo de areglos.

```php
   //noticias.php
 

    //si no concatenemos usamos {} para arreglos indexados  


    function getNoticia($i){
        $noticia1 = [
            'titulo' =>'Noticia',
            'copete' => 'copete',
            'imagen' => '', 
            'cuerpo' => 'noticia'
        ]
        $noticia2 = [
            'titulo' =>'Noticia 2',
            'copete' => 'copete',
            'imagen' => '', 
            'cuerpo' => 'noticia'
        ]
        $noticia3 = [
            'titulo' =>'Noticia 3',
            'copete' => 'copete',
            'imagen' => '', 
            'cuerpo' => 'noticia'
        ]
        $noticias = [$noticia1, $noticia2, $noticia3]
        return $noticia[$i];
    }

```

```php
<!--Index.php-->
<h1>Diario Digital</h1>
<?php

require './noticias.php';
//Esto en realidad iria en otro archivo verNoticas.php
for($i = 0; $i<3; i++){
    $currentNoticia = getNoticia($i);
    echo "
        <div class="noticia">
            <h5>$currentNoticia['titulo']</h5>
            <a href="ver_noticia.php?id={$i}&otro=sfds"> ver noticia<a>
            <!--internamente hacemos un get-->
        </div>
    "

}
<a href="noticias.php">ver noticias</a>



```

```php
//ver_noticia.php
require 'noticias.php';
//obtenemos el parametro get de la url 

$id = $_GET['id'];
$otro = $_GET['otro'];
//Es como decir que $_GET lee la url

$currentNoticia = getNoticia($id);
echo "
    <h1> {$currentNoticia['titulo']} </h1>
    <img src='{$currentNoticia['imagen']}'> 
    <h4>{$currentNoticia['copete']}</h4>
    <p>{$currentNoticia['noticia']}</p>
";

```

siemrpre se recomienda usar require antes que includes. También podemos usar require_once


Para hacer el ver noticia dinamico, necesitamos enviar datos de un cliente a un servidor. Lo hacemos en el link de index.php que generamos por noticia. A una URL podemos pasarle parametros extras que lea el servidor. 