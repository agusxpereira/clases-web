> Vamos a tener un archivo javascript, que es quien consume los datos y termina armando el HTML.

1. Creamos un archivo HTML básico
   - Este va a tenerel formulario y todo lo que teniamos en el ejemplo de los templates, pero además va a tener etiquetas vacías para poder rellenar con JavaScript. 
   - Este seria el HTML básico que luego se descarga con referencia a archivos javascript
2. Creamos nuestro archivo JS 


> Es importante validar que lo que queremos actualizar exista, esto se toma en cuenta en el parcial.
> Por ejemplo en el código el profe esta haciendo:
```php
   //Esto es una llamada en respuesta al método http: (PUT) tareas/:id/Finalizada
   public function finalize($req, $res){
      $id = $req->params->id;//el id de la tarea queda en las queryParams

      $task = $this->model->getTask($id);//obtenemos la tarea que queremos modificar
      if(!$task){//si la tarea no existe
         return $this->view->response("La tarea con el id=$id no existe.",404);
      }

      //validamos los datos
      if(empty($req->body->finalziada)){
         return $this->view->response('Faltan completar los datos', 400);
      }

      //finalizamos
      $this->model->finalizeTask($id);
      //obtengo la tarea modificada 
      $task = $this->model->getTask($id);
      $this->view->response($task, 200);
   }

```

> desde el postman se pasa un sólo dato, quedaria algo como: `{"finalizada": 1}`

> En Javascript hacemos lo mismo: llamamos a los botones editar, agrgando un btnlistener para todos los botones "finalizar", en click llama a `finalizeTask`:


```javascript
   async function finalizeTask(e){
      e.preventDefault();
      try{
         let id = e.target.dataset.task; //obtenemos el id de la tarea

         let response = await fetch(BASE_URL+"tareas/"+id, {//esto llamaria al controller en determinado momento
            method: "PUT",
            headers: {'content-Type':'application/json'},
            body: { finalizada:1 };
         });

         //buscamos la tarea y la modificamos 
         const oldTask = tasks.find(task => task.id === id);
         oldTask.finalizada = 1;
         
         showTasks();

      }catch(e){}

   }
```

Los query params son modificadores del recurso con el que estamos trabajando. Si en un parcial por ejemplo nos piden las tareas ordenadas por nombre, el endpoint de la api deberia quedarnos algo asi:

(GET) `tareas?sortBy=nombre` llama a: `getAll()` en el controlador

Esto se programa "como salga", generalmente es fijarse si existe ese parametro en el controlador, y segun la respuesta podemos agregar algo al sql o organizar como se devuelven los recursos.

Para fijarnos si existen parametros hacemos: `if($req->query->finalizadas)` hacemos algo, nosotros sabemos que filtros existen eso lo definimos nosotros. Por lo que podemos poner un if nomás.

