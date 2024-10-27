Las notas se basan en el parcial, si lo promociamos hay que defenderlo :) 

Hacer autoevaluacions semanales 

no la colguemos con el TPE 

[meet](https://meet.google.com/wze-vugj-nxq)

## tecnologias 
-php 
-mysql 


- Lunes virtuales. 


## Arquitecturas de Sistemas WEB

### Cliente Servidor
- Sistemas Web: Un sistema desarrollado para que funcione en la web. La mayoria se basan en la arquitectura cliente-servidor. 
Las consumen personas y otros sistemas, como API's.

La arquitectura describen la estructura más básica de un sistema. Permite diseñar sin programar un sistema. 
La cleinte-servidor es la que vemos. El cliente hace peticiones al sercidor y envia una respuesta (RESPONSE HTTP). Se comunican con el ptrocolo HTTP.

Define un conjunto pequeño de metodos: 
- GET, PUT, POST, ETC.

- Tiene un encabezado y un cuerpo 

- Códigos de estado 

**"Entrega de una Página Web"**: 
La pagina se la pedimos a un servidor WEB 

No hay diferencias entre las peticiones del cliente para una pagina estatica o dinamica. Es el servidor el que debe decidir que enviar de respuesta según la peticion.Este de ser necesario interactua con archivos y base de datos, para pedirle a una aplicacion que genere un HTML dinámico. 

Hoy en día casi todos los servidores WEB permtien estas llamadas dinamicas. 

Vamos a usar Apache, es software libre, esta en casi todos los servidores, etc. 

En principio hay dos formas de generar una página web: 

- Server side rendering: completo desde el servidor 

- client side: el cliente genera el HTML con los datos enviados del servidor 


Luego hay casos hibridos. 

DEMO: 

```php
<p>"Texto en mi pagina dinámica"<p>
<?php
$a = 3*4;
echo $a
?>

```