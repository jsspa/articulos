{{{
  "title":"[Node] Descubriendo módulos #1",
  "tags": ["node","enchilada","modulos"],
  "category": "descubriendo módulos",
  "date": "01-18-2013",
  "preview":""
}}}

**Descubriendo Módulos** es una serie de articulos con el propósito de mostrar a la
comunidad paquetes/módulos para Node.js. Para que un módulo aparezca en esta sección
debe ser innovador, seguir estandars, y/o ser popular.


En la primer entrega de _Descubriendo módulos_ veremos una serie de middlewares para
express/connect que son interesantes y que posiblemente los necesitaras algún dia.

# Enchilada

Enchilada (GitHub: [shtylman/node-enchilada][enchilada], License: MIT) por Roman Shtylman, dejando a un lado el nombre divertido de este módulo, es 
una herramienta muy interesante al momento de escribir código para el `front-end` sin 
dejar aparte las costumbres de Node (`module.exports`, `require`, etc.).

En otras palabras, `enchilada` te permite escribir código modularizado muy al estilo de
Node, cuando tu código es solicitado él se encargara de hacer lo necesario para que
los distintos `modulos` que tienes esten disponibles al momento de requerirlos.

Por ejemplo, si tienes un archivo con shortcuts a elementos del DOM, tú podrias hacer
lo siguiente:

```javascript
// app.ui.js
module.exports = function (){
  var ui = {
    'el': $('#elemento'),
    'canvas': $('canvas.board')
  }
  return ui
}
```

Y luego en `app.js` simplemente requerir este _módulo_:

```javascript
// app.js

$(document).ready(function(){
  var ui = require('./app.ui')  
  ui.canvas.createContext
})
```

Ahora que se necesita hacer para que esto funcione, simple, montas tu servidor
express y especificas el middleware `enchilada`:

```javascript
var express = require('express')
var enchilada = require('enchilada')

var app = express()

app.use(enchilada(__dirname + '/www')) // asumiendo que `app.js` y `app.ui.js` estan en `www`

app.use('/', function(req, res) {
    res.sendfile(__dirname + '/index.html')
}).listen(8000)
```

Así de simple, reduces a una petición a tu servidor en vez de que sean 2, 4 o más por cada recurso que solicitas.

El código del cliente seria:

```html
<!doctype html>
<head>
</head>
<body>
    <h2 id="elemento">¡Hola!</h2>
    <canvas></canvas>
    <script src="//code.jquery.com/jquery.min.js"></script>
    <script src="/js/app.js"></script>
</body>
```

Si quieres saber más acerca de este módulo visita el [Github del proyecto][enchilada]



  [enchilada]: https://github.com/shtylman/node-enchilada
