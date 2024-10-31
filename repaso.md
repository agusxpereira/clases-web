## seccion 1:

### Arquitecturas WEB

#### Cliente servidor
Es un sistema diseñado para que funcione en la web, en su mayoria la consumen personas y otros sistemas como API's.
Es la estructura más básica de un sistema, el cliente hace peticiones a un servidor y este responde a dicha peticion.

- clientes: browser, otros sistemas, api's, etc.
- comunicacion: protocolo HTTP

El protocolo http define un conunto de métodos:

- GET, PUT,  POST, DELETE, ETC.

Tienen un encabezado(**header**) y un cuerpo(**body**), también código de estados.

#### Entrega de una página WEB

La página se la pedimos a un servidor web. Y es el servidor quien *decide que enviar* según la peticion. De ser necesario interactua con archivos y base de datos para armar dicha página. Casi todos los servidores web permiten estas llamadas dinámicas.

> Esto lo decide el controlador que definimos nosotros para ese servidor, segun el verbo y la url


En principio hay dos métodos formas de generar una página web:
- Server Side Rendering: envía una página completa desde el servidor.
- Client Side Rendering: el cliente "arma" un html básico, recuperando y pidiendo datos al servidor según la accion.

`php` es debilmente tipado. Sus verisones nuevas permiten realizar casteos, ya que definir permite menos errores, lo que es necesario para el backend. La páginas (en ssr) se generan dinámicamente, no tenemo una página por cada noticia o seccion de nuestra web. Se construyen en base a archivos o base de datos y consultas a estas. Podemos pasarle al servidor parametros a través de nuestro URL, ahora vemos como aprovechar esto para hacer un router.

## Seccion 2

### Ruteo

Cada accion que realiza un usuario sobre nuestra web lo hace mediante una peticion http (que por defecto es GET) a través de una url que mapea directamente a un archivo .php (por ejemplo: cuando enviamos un formulario, este como action tiene un archivo php para procesarlo). Para solucionar esto (es un error ya que nos obliga a tener un archivo por cada àccion) podemos realizar un **ruteo** de nuestra aplicacion. 

Es una manera de *configurar la apliacacion* para que acepte urls que no mapeen directamente sobre un archivo (sino que hay un sólo archivo al que apuntan todas las url, y este se encarga de que desiciones tomar).

> Esta configuracion se hace para que todas las peticiones pasen por el archivo `router.php?action=` pero lo vemos un poco más adelante

Este router define algo llamado **tabla de ruteo** que *asocia una url de un método http* con una *accion o parte del sistema especifico*.

|      URL                |    METODO  |      ACCION                   |
|:-----------------------:|:----------:|:-----------------------------:|
|localhost/sumar/a/b      |     GET    |operacion.php::sumar(a,b)      |
|localhost/multiplicar/a/b|     GET    |operacion.php::multiplicar(a,b)|
|localhost/about          |     GET    |about.php                      |
|localhost/login          |    POST    | login.php                     |

Por cada url disponible el router *invoca a un archivo* o a un método de este. Por lo que es el encargado de entender que accion llamo al usuario, leer sus parametros y llamar a quien corresponda.

#### Prettys url

Hasta ahora, nuestras url se verian de la siguiente manera: 
`localhost/noticias.php?id=1`

`localhost/about.php?dev=juan`

Esto nos lleva a dos problemas:
1. Por cada **accion** se necesita un archivo php.
2. Este tipo de urls son malas para SEO.

Las url semanticas son del siguiente tipo:

- Mal:  `http://www.exa.unicen.edu.ar/index.php?hl=es&p=ingresantes` 
- Bien: `http://www.exa.unicen.edu.ar/es/ingresantes`

Son más fáciles de compartir y de entender, además de mejorar el SEO.

El mecanismo para que nuestras url se vean asi es gracias al router, por el cual cada ***solicitud del usuario*** (compeusta por una URL y método HTTP) es dirigida a un ***componente de código*** ecargada de atenderlas.

- Se encarga de determinar el PATH a donde queremos redireccionar
- Implica romper la lógica de "cada url es un archivo".

| Accion(url)                 |       url                     |
|:---------------------------:|:-----------------------------:|
| /home                       |showHome();                    |
| /about                      |showAbout();                   |
| /about/:dev                 |showAbout(:dev);               |
| /noticia/:id                |showNoticia(:id);              |

El router es el elemento principal que atiende a TODOS los request. Este archivo *encapsula* el comportamiento del *componente ruteador*.
> este comportamiento seria atender las peticiones y controlar el flujo de la app

Este comportamiento es el siguiente: lee una ***acción*** y una lista de **parámetros** -> :**action**/[:*a*/:*b*].
-> Recibe la siguiente url, que contiene a la accion: `route.php?action=noticia/1`

> ahora si deberiamos reescribir la urls para que queden de la siguiente manera: /route.php?action=about/juan & /route.php?action=noticia/1
Para por fin hacerlas pretty usamos las reglas apache, para que cada url pase primero por el router.php.

#### datos estaticos
Este problema surge cuando desde determinado lugar, se intenta acceder a una imagen o recurso de manera desde una ruta relativa, pero que no se puede alcanzar desde esa archivo. 

Debemos agregar un BASE_URL en el router, que seria la carpeta raíz, y ponerla en el template html para que siempre sepa cual es la ruta para acceder a los archivos.

## clase 3

Para operar con datos debemos debemos conectarnos a mysql (que seria el gestor de sistemas de base de datos) con php. Este motor es un servicio independiente.

Ambos son servicios independientes en el servidor, por un lado tenemos instalado php y por otro el gestor, ambos pueden comunicarse.

Para conectarse a una base de datos, generalmente se siguen estos pasos:

1. abrimos la conexion
2. ejecutamos la consulta
3. obtenemos los datos
4. cerramos la conexion

Para poder hacer esto con casi cualquier base de datos usamos PDO, que es una capa de abstraccion entre la lógica y los datos, lo que hace que podamos trabajar con casi cualquier base de datos.

#### 1) abrimos la conexión

`$db = new PDO('mysql:host=localhost;dbname=test_db;charset=utf8', 'root', '');`

#### 2) ejecutamos la consulta

`$sentence = $db->prepare("select * from tarea");`

`$sentence->execute();`
#### 3) obtenemos los datos para generar un html
$tareas = $sentence->fetchAll(PDO::FETCJ_OBJ);
foreach($tareas as $tarea){
    echo $tarea->nombre;
}
#### 4) Con PDO no es necesario cerrar la conexión

fetchAll nos trae un arreglo de las tuplas, o de las filas/registros de la base de datos.

también se podria hacer algo así pero se considera mala practica

```php
while($fila = $sentencia->fetch(PDO::FETCH_OBJ)){
        echo '<li>' . $fila->titulo . ': '. $fila->descripcion.'.'. '</li>';
}
```

- PDO::FETCH_ASSOC: devuelve un array asociativo por los nombres de las columnas del conjunto de resultados.
- PDO::FETCH_OBJ: devuelve un objeto anónimo con nombres de propiedades que se corresponden a los nombres de las columnas devueltas en el conjunto de resultados.
- PDO::FETCH_BOTH: devuelve un array indexado tanto por nombre de columna, como numéricamente.
- PDO::FETCH_NUM: devuelve un array indexado por el número de columna tal como fue devuelto en el conjunto de resultados, comenzando por la columna 0.


Para insertar es el mismo procedimiento, solamente que los valores a insertar en la consulta toman el valor de '?' y cuando ejecutamos el insert, la funcion `->execute()` espera un arreglo con los valores.

### Base de datos

Una base de datos es una herramienta para recompilar y organizar información. Es un contenedor para almacenar tablas que guardan datos interrelacionados.


### Persistencia
Es la accion de *preservar la informacion* de forma permanente y a su vez *recuperar* la misma que pueda ser nuevamente utilizada.

Sobre los datos podemos realizar determinadas operaciones

- altas
- bajas
- modificaciones o actualizaciones
- consultas

> CRUD

#### datos
un dato es el "estado" que toma un atributo.

- Se llama **campo** a cada atributo de la tabla. Por ejemplo: nombre y edad son campos.
- Se llama **registro** al conjunto de campos que definen un elemento de la tabla.

- filas = registros (*Un registro de atributos y estado*)
- columnas = atributo/campo



#### Modelo entidad-relacion

Es un modelo semantico que describe los requerimientos de datos de un sistema. Elementos del MER:

- Entidad: objeto *real o abstracto* de la vida real del cual se requiere almacenar informacion.
- Relacion: asociacion entre entidades
- Atributos: caracteristicas que describen las entidades y relaciones

#### claves foraneas

Es un ***atributo o atributos*** que establece un vinculo lógico entre tablas. Por lo general, asocia un campo de una tabla con la clave primaria de otra tabla o tablas. En este caso los atributos `id_conductor` y `id_usuario`.

## unidad 4

### MVC

es una solucion para desacoplar el código de programas dónde toda la lógica , el acceso a datos y la interfaz grafica conviven en archivos sin ningún tipo de separacion clara.

Divide la lógica en **tres elementos** interrelacionados

- Modelo: acceso a los datos
  - protege y persiste los datos del usuario
  - asegura la **integridad** de los datos
  - provee métodos para insertar. consultar, actualizar y eliminar los datos (CRUD)
- Vista: Interfaz grafica
  - Presenta la información al usuario.
  - Permite al usuario **interactuar con la aplicacion**
- Controlador: coordina entre vista y modelos
  - Controla y coordina el flujo del ala plaicacion
  - Procesa las solicitudes del usuario
  - Valida la entrada de datos del usuario.

Cada compenente es una clase

## unidad 5

### Plantillas

Son archivos que se utilizan para separar la lógica del programa y la **presentacion** del contenido en dos partes independientes.
Se enfocan en tener plantillas rápidas, poco código y mantenibles.
Php nos ofrece alternativas a las estructuras de control clásicas.

#### La lógica 
es la parte del código que realiza todo lo referido a la obtencion, almacenamiento, y procesamiento de los datos para entregarlos a una vista que sabe como visualizarlos. Se dice que es el detrás de escena necesario para poder presentar los datos en la pantalla.

#### .phtml
es una extension aceptada por php. Usa una combinacion de etiquetas HTML y etiquetas de plantilla para formatear la presentacion del contenido. 

Se usa el formato: `<?= $var >`

## unidad 6 - rest api

### SSR - Server Side Rendering

Todos los recursos (como /tareas) están *alojados o son creados* dentro del servidor. Cuando un usuario realiza una solicitud, este devuelve un HTML final completo para que lo muestre el browser.

### Client Side Rendering

En lugar de obtener un sitio completo, el servidor devuelve un html vacío y el render se completa del lado del servidor usando JavaScript.

Toda la lógica, tanto como templates, ruteo y obtencion de información es manejada en el lado del cliente.
- Usado para hacer SPA's.
- Muchos frameworks disponibles

> existe un enfoque hibrido 


### web services

Son componentes de una aplicación especificos para intercambiar informacion entre aplicaciones.

Permite la interoperabilidad entre distintas plataformas y sistemas por medio de protocolos estandares y abiertos.

Actualmente la mayoria de los sistemas utilizan servicios web.

- Los sistemas se comunican entre ellos
- compartes informacion

> Uno es consumidor del servicio, otro es proveedor.

Existen diferentes **protocolos y arquitecturas** de servicios. Los principales son:

- **SOAP** - Simple Object Access Protocol: muy usado en sistemas coorporativos.
- **REST** - Representational State Transfer: muy usado en la internet.

> ... hay más

### API

- Es una *interfaz*.
- Permite la utilizacion desde el exterior del sistema.
- Definen datos y cómo acceder a ellos.
- Implementada como:
  - Procedimientos
  - Funciones
  - Objetos y métodos
  - Servicios web

### API - REST
Se apoya totalmente en el protocolo HTTP. Proporciona una API que utiliza cada uno de los métodos del protocolo.

- Es un tipo de arquitectura más natural y estandar para crear APIs para servicios orientados a internet.
- Una URI representa un **recurso** al que se puede acceder o modificar mediante los métodos del protocolo HTTP (POST, GET, PUT, DELETE).


**REST** define un conjunto de principios arquitectonicos por los que se pueden diseñar **Servicios Web** que se centren en los *recursos de un sistema*.

Si la implementacion concreta sigue al menos estos principios básicos de diseño, se lo conoce como **RESTful Web Services**:

- **Interfaz unificada (Recurso + URI + Verbo HTTP)**
> una uri es una cadena de caracteres que identifican los recursos

- **Cliente-servidor**
- **Stateless**

**Recurso**: Cualquier información que se pueda nombrar puede ser un ***recurso***.
> *Usuario*
> 
> Factura
> 
> Cliente

**URI (endpoint)**: Son los puntos de entrada con las que el cliente accede a un recurso.

>http://www.example.com/api/*usuario*
>http://www.example.com/api/*usuario*/5

**Método del recurso**: se utilizan métodos HTTP de manera explicita junto a cada recurso para realizar la transaccion deseada.

> GET
> 
> POST
> 
> PUT
> 
> DELETE

### API REST- respuestas

Cada solicitud a una API REST debe responder con **código de respuesta**.

Inidican el resultado de una solicitud:
- 1xx: Informational
- 2xx: Success
- 3xx: Redirection
- 4xx: Client Error
- 5xx: Server error

### Protocolo HTTP

Es uno de muchos protocolos de transferencia de datos (FTP, DHCP), se basa en el modelo cliente-servidor. Permite transeferir distintos tipos de recursos. También es Stateless (las request son independientes).

> Para que se puedan transferir datos debe existir una **comunicacion** entre el cliente y el servidor
> - El servidor tiene una direccion IP y un puerto dónde escucha por pedidos entrantes (localhostt)
  - Cuando el cliente conoce la IP y el puerto puede enviar un request

#### partes de un request
- HTTP method: indica que tipo de operación quiere el cliente.
- Path: especifica la ubicacion de los datos que se necesiten (ejemplo: */horarios/* en la página de exactas)
- Version: especifica que version de HTTP está usando el cliente
- Headers: contiene informacion importante del server (basado en key-values)
- body: algunos tipos de request (ej. *POST*) lleva un payload (ej. datos de un forum)

#### métodos

- GET: recuperación de informacion de un recurso especifico
- HEAD: es similar a un GET excepto que el servido no debe enviar un body en la respuesta
- POST: *crea datos* de un recurso especificado.
- PUT: actualiza datos de un recurso
- DELETE: elimina el recurso especificado
- OPTION: solicitar informacion sobre las opciones de comunicacion disponible para el recurso de destino
- y otros.. PATCH, COPY, LINK, UNLINK, PURGE, LOCK, UNLOCK, etc.

> El body suele ser lo más importante, ya que contiene la informacion o datos que el cliente solicito.

### $req

> si no me equivoco, al objeto $req lo crea el controlador al momento de ejecutar un verbo por ejemplo

### diseño de endpoints


>          /recruso
- GET `api/tareas`
    > Devuelve todas las tareas.
- POST `api/tareas`
    > Crea una nueva tarea.
- GET `api/tareas/:id`(api/tarea/123)
    > Devuelve una tarea especifica.
- PUT `api/tareas/:id`
    > Edita la tarea, sustituyendo la información enviada.
- DELETE `api/tareas/:id`
    > Elimina una tarea específica.

### arquitectura MVC para rest

> el controlador es definido por la API

Ruteamos en base al **recurso** y **método HTTP**.

| URL       | Verbo    |Controller         | Método               |
|:---------:|:--------:|:-----------------:|:--------------------:|
| tareas    |  GET     | ApiTaskController | obtenerTareas()      |
| tareas    |  POST    | ApiTaskController | crearTarea()         |
|tareas:/id |  GET     | ApiTaskController | obtenerTarea($id)    |
|tareas:/id |  DELETE  | ApiTaskController | eliminarTarea($id)   |
|tareas:/id |  PUT     | ApiTaskController | actualizarTarea($id) |

> 1. Redirigimos la solicitud a router.php

> Buena practica: agregamos prefijo **api/** en la URL

```
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^api/(.*)$ route.php?resource=$1 [QSA,L,END]
</IfModule>
```
### APIView

Es una vista común para todos los servicios.
- Maneja el **código de respuesta**
  > 404, 200, 220, etc.  
- Devuelve la informacion en formato JSON.
  > La informacion del modelo

Resultado esperado:

`Reponse Content Type`
`aplication/json`

```json
    [
        {
            "id_tarea": "integer",
            "tarea": "string",
            "realizda": "boolean"
        }
    ]
```
### implementacion de una api view
> es la api view quien responde lo que muestra por ejemplo la consola o el postman, es mucho más importante que la vista que teniamos en el tpe
```php
class APIView() {
    
    public function reponse ($data, $status){
        
        header("Content-Type: application/json");
        header("HTTP/1.1" . $status . " " . $this->_requestStatus($satus));

        echo json_encode($data);
        //si no me equivoco esto es lo que muestra cuando hacemos un llamado en postamn

    }
    public function _requestStatus ($code){
        $satus = array(
            200 => "OK",
            404 => "No found",
            500 => "Internal Server Error",
        );

        return (isset($status[$code])) ? $status[$code] : $satus[500];
        //si esta seteado lo devuelvo, sinó devuelvo 500
    }

}

```
### ejemplo de como obtener una tarea

> La api tiene que perimitir traer una tarea:

`/api/tareas/:ID`

#### TareasAPIController

```php

    function get($req){
        if(empty($req->$params->id)){
            $tareas = $this->model->getTareas();
            return $this->view->response($tarea, 200);
        }else{
            $tarea = $this->model->getTarea($req->params->id);
            if(!empty($tarea)){
                return $this->view->response($tarea, 200);
            }//else ??
        }
    }

```


### ¿como enviamos datos?

### ¿Cómo se envian los datos?

> Agregamos tarea (recurso **tareas**)

> verbo: (POST) recurso: api/tareas

Para trabajar con APIs REST, esperamos los datos en formato JSON

Para la estructura del dato, **usamos la misma de salida**:

```json
//así armariamos al dato que queremos ingresar
{
    "titulo": "Tarea API Rest",
    "descripcion": "Una tarea creada desde la API",
    "prioridad": 5
}
```

> esto lo podemos hacer por ejemplo desde postman o desde un formulario mediante AJAX

### Modificacion del recurso (PUT)

> Lógica

- Similar al **POST**
- En lugar de **crear** una tarea, vamos a **modificar** una que ya existe.
- Vamos a necesitar al :ID de la tarea a modificar.

> ¿Cuál es la URL que vamos a crear?

> Combina parametros y el acceso al body del request

```php
//TasApiController.php
public function updateTask($req){
    
    $task_id = $req->params->id;
    //el id de api/tareas/:ID
    $task = $this->model->getTask($task_id);

    if($task){
        $titulo = $req->body->titulo;
        $descripcion = $req->body->descripcion;
        $finalizada = $req->body->finalizada;
        $tarea = $this->model->updateTask($task_id, $titulo, $descripcion, $finalizada);
        $this->view->response("Tarea id=$task_id actualizada con éxito", 200);
    }else{
        $this->view->response("Task id=$task_id not found", 404);
    }

}

```

### Parametros GET

`api/tareas?sort=prioridad&order=asc`

Por parámetro GET recibimos el valor de "sort" y "order"
- Devuelve el arreglo de tareas ordenado por prioridad ascendente

`/api/tareas/?pending=true`
- Por parámetro GET recibe el valor de "pending"
- Devuelve el arreglo de tareas que **NO** están finalizadas

> estas serian las famosas queryParams, cuando hacemos un get por ejemplo en esa direccion, sigue el camino del router definido para (GET): `api/tareas`, pero guarda los parametros en el objeto $req que se crea.


### Subrecurso

`api/tarea/1/descripcion`

- Devuelve sólo la descripcion de la tarea
    
  En este caso, finalizada no se considera un sub-recurso en el sentido estricto de REST, sino más bien un atributo o propiedad del recurso tarea. La estructura tareas/:id/finalizada da la apariencia de un sub-recurso, pero en realidad estás trabajando con el estado de la tarea identificada por id.

    ¿Por Qué No Es un Sub-Recurso?
    En un esquema REST puro, un sub-recurso suele representar un recurso hijo o relacionado que existe de forma independiente. Ejemplos típicos de sub-recursos serían:

    ´tareas/:id/comentarios´ para manejar los comentarios de una tarea específica.
    ´tareas/:id/adjuntos´ para archivos o adjuntos asociados a la tarea.
    
    En cambio, finalizada es simplemente un atributo booleano de la tarea, no un recurso independiente que tenga su propia identidad.

    Alternativas RESTful para Modificar el Atributo finalizada
    Dado que finalizada es un atributo de la tarea, una alternativa común en APIs RESTful sería actualizar ese atributo con un endpoint que acceda directamente al recurso:

    PATCH sobre la Tarea Directamente:
    > (PATCH) ´/tareas/:id´
    > ´{ "finalizada": true }´
    >
    > O bien

    Mantener la Riqueza Semántica con PUT/PATCH: Si aún prefieres una estructura que especifique el cambio de estado, otra opción semántica es:

   > (PATCH) ´/tareas/:id/estado´
   >
   Con un cuerpo:

  Con un cuerpo:

    json
    ´{ "estado": "finalizada" }´
