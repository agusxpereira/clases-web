# #1 Arquitecturas web

En este video se  habla de la arquitectura preponderante para casi cualquier sistema WEB: La arquitectura cliente servidor. Se explica la generacion de página dinámicas y como estas permiten que por ejemplo, al dar like a un post en una red social, este repercuta de inmediato en la publicacion original.

Lo primero a tener en cuenta es la separacion entre el fronted y el backend:

***Frontend***: Es el encargado de interactuar con el usuario, de presentarle datos. Existe en los celulares o computadoras del usuario.
***Backend***: Permite que las páginas o aplicaciones sean dinámicas.

Ambas partes del Sistema interactúan gracias a la arquitectura cliente-servidor. El cliente hace una peticion al servidor (que está preparado para recibir multiples peticiones) y este genera una respuesta dinámicamente.

*Uml*:
- Nodos: hardware.
- Componentes: software.

Cliente y servidor se comunican mediante una interfaz (punto de entrada que comunica a dos sistemas). Esta comunicacion es posible gracias al protocolo HTTP.

*¿Como se generan las respuestas dinamicas?*: Esto es posible gracias a la programacion server-side. Que es el componente principal de los sitios dinamicos. Primero recordemos como se genera una pagina estatica en una estructura cliente servidor. El cliente realiza una peticion, en este caso un archivo, y el servidor devuelve dicho archivo. En este escenario, la funcion del servidor es muy simple.

En el caso de los sitios dinamicos, el servidor sabe que se pide una pagina dinamica, por lo que se invoca a la applicacion web que convive en el servidor web. Es la aplicacion la que nosotros vamos a escribir para que genere respustas dinamicas, que no es más que un HTML nuevo generado de manera dinamica con archivos que ya existen y registros en bases de datos.