# syncJSON - POC

SyncJSON es una prueba de concepto, con la idea de ver las distintas formas para que un documento HTML se rellena con datos a partir de un JSON (ya sea remoto o local) y usando JavaScript nativo.


### Prueba 1 - Usando un identificador o el tag name:

En esta prueba básica obtenemos todos los identificadores e insertamos el valor que coincide con el identificador del JSON. Insertar valores simples no supone problemas, por otra parte, es algo más complejo tenemos un Array en el JSON y necesitamos mostrarlo con una estructura predefinida. 

En nuestro caso tenemos una cesta con varios productos, la idea es que se muestre siguiendo una estructura que nosotros predefinimos y por cada producto se genere dicha estructura con sus datos. 

En los casos que necesitamos meter valores a los atributos no está implementado. Siguiendo el ejemplo de la cesta, podríamos añadir un nuevo valor al JSON. Donde se especifica la dirección de la imagen del producto. 


Fichero JSON 
```
data = {
  usuario: {
    id: 129821712812791287127774747484848,
    nombre: "John",
    apellidos: "Doe",
    identificacion: "0000000000A",
    genero: "masculino",
    carrer: "FullStack Developer"
  },
  cesta: {
    productos:[
        {descripcion: "platano"},
        {descripcion: "coca-cola"},
        {descripcion: "pizza"}
    ],		
  },
}
```
Fichero HTML 
```
<body>
  <h1 name="usuario.nombre"></h1>
  <ul name="carrito.productos">
    <li name="descripcion"></li>
  </ul>
</body>
```

Fichero JavaScript
```
function getArrayJson(schema, datos){   
	var estructura = ""
					
	datos.forEach(e => {
		backup = "";
		backup = schema.cloneNode(true);
		Object.keys(e).forEach(key => {
			backup.querySelector('[name=' + key +']').innerHTML += e[key];
		});
		estructura += backup.innerHTML;
	}); 
			
	return estructura
}

function getJsonValue(json, path) {
	return path.split('.').reduce((p, c) => p && p[c] || null, json)
}
		
ids =  document.querySelectorAll('*[name]')
	ids.forEach(e => {
		var value = getJsonValue(data, e.getAttribute('name'))
		if(!Array.isArray(value)){
			e.innerHTML  += value  
		} else {
			e.innerHTML = getArrayJson(e, value)
	}
							  
});
```




### Prueba 2 - Remplazando los identficadores por el valor del JSON:

La idea en esta prueba consiste en sustituir el valor del identificador por el valor del JSON. Manteniendo la posición donde se encuentra, es decir podemos insertar el identificador en un atributo del elemento, insertar el valor entre etiquetas. etc 


Fichero JSON 
```
data = {
  usuario: {
    id: 129821712812791287127774747484848,
    nombre: "John",
    apellidos: "Doe",
    identificacion: "0000000000A",
    genero: "masculino",
    carrer: "FullStack Developer"
  },
  cesta: {
    productos:[
        {descripcion: "platano"},
        {descripcion: "coca-cola"},
        {descripcion: "pizza"}
    ],		
  },
}
```
Fichero HTML 
```
<body>
  <h1 id="usuario.id">usuario.nombre</h1>
  <ul id="carrito.productos">
    <li>descripcion</li>
  </ul>
</body>
