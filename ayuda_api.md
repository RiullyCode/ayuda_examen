# Ayuda API

## Creación de los archivos necesarios:
Lo primero que haremos será crear los archivos necesarios para realizar el proyecto. Creremos un html principal en el cual se listaran los elementos de la api y luego otros 3 más, los cuales tendran formularios para añadir, eliminar y modificar los datos de la api. Si se quiere hacer uso de las plantillas de este archivo es recomendable que de nombre para los archivos se pongan los siguientes:

Para la vista principal:
```
principal.html
```

Para la vista del formulario de añadir:
```html
añadir.html
```

Para la vista del formulario de eliminar:
```html
eliminar.html
```

Para la vista principal:
```html
modificar.html
```

## Creación del menú
Para poder navegar entre la vista de los elementos y los formularios se creará un menú con bootrstrap, el link de bootrstrap es el siguiente:
```html
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
```

ahora cogeremos un menú de bootstrap, y lo adaptaremos a las vistas que tenemos creadas, quedaría tal que así:

```html
   <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Conexión a API</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="./principal.html">Conexión API</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item active">
              <a class="nav-link" href="./agregar.html">Agregar</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="./eliminar.html">Borrar</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="./modificar.html">Modificar</a>
            </li>
          </ul>
        </div>
      </nav>
```
## Creación de los formularios

Ahora pasaremos a la creación de los formularios necesarios para realizar el proyecto, el cual estará compuesto por 3 formularios, una en cada vista.

En la de `agregar.html` irá el siguiente:

```html
      <form name="formulario-agregar" action="https://bbdd.lhdevsolutions.net:50500/api/personas" onsubmit="enviar(); return false;">
        <div class="container" style="margin-top: 10px;">
          <div class="mb-3">
            <label for="dni" class="form-label">DNI</label>
            <input type="text" class="form-control" id="dni" name="dni">
          </div>
          <div class="mb-3">
            <label for="name" class="form-label">Nombre</label>
            <input type="text" class="form-control" id="name" name="name">
          </div>
          <div class="mb-3">
            <label for="surname" class="form-label">Apellidos</label>
            <input type="text" class="form-control" id="surname" name="surname">
          </div>
          <button type="submit" class="btn btn-primary">Enviar</button>
        </div>
      </form>
```

IMPORTANTE ACORDARSE DE MODIFICAR TODOS LOS ELEMENTOS ACORDE A LA API

En la de `eliminar.html`:
```html
    <form name="eliminar" onsubmit="nombre_funcion(); return false;">
  <div class="mb-3">
    <label for="dni" class="form-label">DNI:</label>
    <input type="text" class="form-control" id="dni" name="dni" required>
  </div>
  <button type="submit" class="btn btn-danger">Eliminar</button>
</form>
```

En la de `modificar.html`:
```html
        <form name="formulario-buscar">
        <div class="container" style="margin-top: 10px;">
          <div class="mb-3">
            <label for="dni" class="form-label">DNI</label>
            <input type="text" class="form-control" id="dni" name="dni">
          </div>
          <button type="button" class="btn btn-primary" onclick="buscarPersona()">Buscar</button>
        </div>
      </form>
    
      <form name="formulario-actualizar">
        <div class="container" style="margin-top: 10px;">
          <div class="mb-3">
            <label for="name" class="form-label">Nombre</label>
            <input type="text" class="form-control" id="name" name="name">
          </div>
          <div class="mb-3">
            <label for="surname" class="form-label">Apellidos</label>
            <input type="text" class="form-control" id="surname" name="surname">
          </div>
          <button type="submit" class="btn btn-primary">Actualizar</button>
        </div>
      </form>  
```

## Funciones de cada formulario:

Funcion para el agregar.html:

```html
    <script>
        function enviar() {
          const formulario_agregar = document.forms["formulario-agregar"];
          const formData = new FormData(formulario_agregar);
          persona = Object.fromEntries(formData.entries())
          persona.dni = persona.dni;
          persona.nyap = String(persona.name + " " + persona.surname);
          console.log(persona);
          const url = formulario_agregar.action;
          enviarJson(persona, url);
        }
      
        function enviarJson(persona, url) {
          fetch(url, {
              method: "POST",
              body: JSON.stringify(persona),
              headers: { "Content-type": "application/json; charset=UTF-8" }
            })
            .then(response => response.json())
            .then(json => console.log(json))
            .catch(err => console.log(err));
        }
      </script> 
```

Función para eliminar.html:
```js
        function eliminar() {
          const formulario_eliminar = document.forms["formulario-eliminar"];
          const dni = formulario_eliminar.elements["dni"].value;
          const url = `https://bbdd.lhdevsolutions.net:50500/api/personas/${dni}`;
          fetch(url, {
            method: "DELETE"
          })
          .then(response => response.json())
          .then(json => console.log(json))
          .catch(err => console.log(err));
        }
```

funcion para modificar:
```js
      function buscarPersona() {
          const dni = document.getElementById("dni").value;
          const url = `https://bbdd.lhdevsolutions.net:50500/api/personas/${dni}`;
    
          fetch(url)
            .then(response => response.json())
            .then(json => {
              if (json.nyap) {
                const nyap = json.nyap.split(" ");
                document.getElementById("name").value = nyap[0];
                document.getElementById("surname").value = nyap.slice(1).join(" ");
              } else {
                alert("No se encontró la persona con ese DNI");
              }
            })
            .catch(err => console.log(err));
        }
    
        function actualizarPersona() {
  const dni = document.getElementById("dni").value;
  const name = document.getElementById("name").value;
  const surname = document.getElementById("surname").value;
  const nyap = name + " " + surname;
  
  // Construir el objeto de persona actualizada
  const personaActualizada = {
    dni,
    nyap
  };

  // Enviar la solicitud PUT para actualizar la persona
  const url = `https://bbdd.lhdevsolutions.net:50500/api/personas/${dni}`;
  fetch(url, {
      method: "PUT",
      body: JSON.stringify(personaActualizada),
      headers: { "Content-type": "application/json; charset=UTF-8" }
    })
    .then(response => {
      if (!response.ok) {
        // Si la respuesta no es "ok" (200), lanzar un error con la respuesta
        throw response;
      }
      // Si la respuesta es "ok", devolver la respuesta como JSON
      return response.json();
    })
    .then(json => {
      console.log(json);
      // Mostrar un mensaje de éxito al usuario
      alert(`La persona con DNI ${dni} se ha actualizado correctamente.`);
    })
    .catch(err => {
      // Si hubo un error, mostrar detalles sobre el error en la consola
      err.json().then(json => console.error(json.errors));
    });
}
```

