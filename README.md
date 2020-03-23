# syncJSON - POC

SyncJSON es una prueba de concepto, con la idea de ver las distintas formas para que un documento HTML se rellena con datos a partir de un JSON (ya sea remoto o local) y usando JavaScript nativo.


### Prueba 1 - Usando un identificador o el tag name:

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
