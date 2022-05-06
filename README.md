# Desafio "Javascript en práctica"

![captura presentación][0]

|Bootcamp 2022 Modulo 4|Introducción a la programación en Javascript|
|----|-----|
|**Unidad n°1**|Introducción al lenguaje Javascript|
|**Día Bootcamp**|45|
|**Día Modulo**|6/15|



El desafío 'Javascript en práctica' consiste en una serie pequeña de 3 ejercicios que requieren la aplicación de distintas funcionalidades de Javascript en códigos HTML ya preparados:

1. Aplicación de patrones regex.
2. Utilización de eventListeners y selectores del DOM.
3. No se bien. Este ejercicio no aporta nada diferente a los 2 anteriores.
 
<hr>

## REFERENCIAS *'cause we nerds... love them!! :)*

* [Tutorial de expresiones regulares en MDN][6]
* [CheatSheet de expresiones regulares en MDN][7]

<hr>

## REQUERIMIENTOS

|N°|Estado|Requisito|Puntaje|
|:-------:|:------:|:------:|:------:|
|1|OK|Validar formulario de contacto con regex `onsubmit`|N/A|
|2|OK|Crear un selector de colores con evento `click`|N/A|
|3|OK|Dar funcionalidad de sumar y restar a la calculadora|N/A|

<hr>

## IMPLEMENTACIÓN

* En términos generales, se ha implementado una interfaz muy simple a partir de enlaces en un `<main>` aplicando una transición para crear un menú de acceso a cada ejercicio, los que han sido tratados como archivos html independientes que comparten una sola hoja de estilos. Se ha centrado utilizando la propiedad `place-content` con CSS Grid.

![captura3][8]

* Los estilos generales se han trabajado utilizando Sass, y aprovechando que las líneas vacías no son consideradas en la compilación, sigo con la práctica de separar el código CSS dentro de cada regla según su ámbito de acción. No hay nuevas propiedades en esta ocasión. Solo sigo practicando el uso de CSS Grid, Flex, y otros como sombras, colores rgba, bordes y tamaños dinámicos.

<hr>

* El ejercicio 1 se ha realizado dentro de una sola función general `valida()` para aislar el scope. Dentro de esta se usan pequeños métodos de arreglos y una sola función para validar los campos y evitar procesos repetitivos. La núcle de la funcionalidad es:  

```javascript
function validaRegex([campoValor, campoString, campoError]) {
  if (!campoValor) {
    campoError.innerText = `El ${campoString} es requerido`;
    validacionFlag = validacionFlag && false; 

  } else if (/^[^ \d][a-zñáéíóú ]+[a-zñáéíóú]+$/i.test(campoValor)) {
    validacionFlag = validacionFlag && true; 

  } else {
    campoError.innerText = `Solo utilizar letras y espacios. No comenzar/terminar con espacio(s)`;
    validacionFlag = validacionFlag && false;
  }; 
}
```

<hr>

* El ejercicio 2 es el más simple de todos. Los botones están todos dentro de un wrapper y cada boton tiene harcodeado con estilo inline el color de fondo. Por lo tanto, solo basta agregar un eventListener al wrapper de los botones y capturar el valor del color dentro del atributo `style` para aplicarlo como color de fondo al cuadro grande. La función medular es: 

```javascript
function colorea(e) {
  const color = e.target.getAttribute('style').slice(18, 25);
  caja.style.background = color;
}
```

<hr>

* El ejercicio 3 es el más feo. No tiene nada particularmente atractivo para aprender. Su función central es una que recibe los `value` de 2 `input`'s además del valor del atributo `id` de un boton para decidir si los valores deben sumarse o restarse. Para practicar un poco del [camino del ninja 🤣][10] he utilizado un operador ternario: 

```javascript
function operar(valor1, valor2, btn) {

  console.log(valor1, valor2, btn);

  resultado.innerText = 
    btn === 'btn-sumar'
      ? `${valor1 + valor2}`
      : valor1 < valor2 
        ? `0`
        : `${valor1 - valor2}`
}
```

<hr>

## NOTAS

- **CURIOSIDAD PARA INVESTIGAR Y APRENDER:** Si hago un console.log de los parámetros pasados a la función `validaRegex(arg1, arg2, arg3)`  justo en la primera línea de este bloque, igualmente me mostrará el valor final del párrafo, esto es, con el valor para la propiedad `innerText` que se le asigna dentro de la ejecución del condicional if que le sigue. Esto es super extraño para mí. Pasa tan rápido que el inspector de elementos muestra generalmente el mismo *timestamp* así que este no permite resolver qué sucede primero. Intenté colocar unos `console.time()` para verificar esta situación y lo único que logro observar es que el código dentro del if se ejecuta hasta cerca de 10 veces más rápido que la impresión de los `console.log( )` anteriores. ¿Sucede que simplemente el console log que se hace a los elementos `<input> y <p>` es sobre elementos "vivos" que se actualizan unos microsegundos luego de ser pintados vacíos en la consola? ¿tendrá que ver con el *eventloop* y que las tareas de pintar en la consola son más demandantes por lo que cuando finalmente se pinta, el condicional que le sigue ya está resuelto? El código y una captura de las *Dev Tools*: 

```javascript
function validaRegex([campoValor, campoString, campoError]) {
  console.time('timelog para valores de parámetros validaRegex(valor, string, error)');
  console.log(campoValor, campoString, campoError);
  console.timeEnd('timelog para valores de parámetros validaRegex(valor, string, error)');
  
  console.time('timelog para condicional if');
  if (!campoValor) {
    campoError.innerText = `El ${campoString} es requerido`;
    validacionFlag = validacionFlag && false; 

  } else if (/^[^ \d][a-zñáéíóú ]+[a-zñáéíóú]+$/i.test(campoValor)) {
    validacionFlag = validacionFlag && true; 

  } else {
    campoError.innerText = `Solo utilizar letras y espacios. No comenzar/terminar con espacio(s)`;
    validacionFlag = validacionFlag && false;

  };
  console.timeEnd('timelog para condicional if');
  
  console.log(`Saliendo de validaRegex() para "${campoString}" con validacionFlag = ${validacionFlag}`);
}
```

![captura de situacion con console.log y condicional if][1]

<hr>

- **OTRA CURIOSIDAD ENTRETE:** los elementos "nombrados", por ejemplo aquellos con atributo `id` no vacío (pero no limitados a este caso solamente), son añadidos al objeto `window` como propiedades. La **WHATWG** recomienda [no confiar en estas propiedades][2] como variables globales ni selectores CSS, pues indica que la implementación  de este comportamiento puede cambiar a través del tiempo. Una lectura en [Stackoverflow][3] sobre el tema con una referencia circular al enlace de la WHATWG presentado unas palabras antes. En las imágenes siguientes se observa que solo hay 3 variables declaradas en el script, mientras que existen al menos 7 valores `id` en el código HTML. Luego puede observarse como en la consola de las herramientas de desarrollo se pueden invocar los valores tanto de las variables creadas, algo lógico, como también las de los elementos asociados a cada id. Como 'yapa' se observa la dificultad añadida de crear nombres de id con el signo '-'. 

![captura de id's como variables 1][4]
![captura de id's como variables 2][5]

<hr>

- **Una regla CSS para centrar** que he utilizado por su comodidad y sentido común al leer el código es `place-content: center` junto con `display: grid`. El único pero es que tiene que declararse la altura del elemento para que funcione verticalmente (lo que tiene todo el sentido del mundo por lo demás). 

```css
body {
  display: grid;
  place-content: center;
  height: 100vh;
  /* ... */
}
```

<hr>

- Algo simpático y ligeramente desafiante que me sucedió al intentar utilizar una sola función para validar todos los campos. Mareado al medio del estudio terminé con una validación muy intrincada que aún así funciona y de paso aprendí a utilizar condicionales de asignación del tipo `let variable = value && true|false` Este tipo de asignación no es la misma que `let variable &&= true | false` [Leer sobre esto][9] en MDN.

- **Finalmente, un FLAG** solo para recordarme que dentro de un flujo de ejecución, todo se pueden considerar tipos cambiando dinámicamente debido a la naturaleza de Javascript, y es muy importante entender cuando utilizo métodos de las webapis o mis propias funciones tener claridad de los tipos de cada variable e identificador dentro del código. Un ejemplo acá fue la siguiente función para limpiar el contenido de algunos elementos que no funciona bien en su primera forma porque la mitad de los elementos no tienen un atributo `value` válido. La segunda forma es la adecuada: 

```javascript
// PRIMERA FORMA ERRÓNEA. LOS 4 PRIMEROS ELEMENTOS SON <p>'s,
// MIENTRAS QUE LOS 3 SIGUIENTES SON <input>'s

[ resultado, 
  errorNombre, 
  errorAsunto, 
  errorMensaje, 
  nombreValor, 
  asuntoValor, 
  mensajeValor ].forEach(e => e.value = null);

// CORREGIDO:

[ nombreValor, 
  asuntoValor, 
  mensajeValor ].forEach(e => e.value = null);

[ resultado, 
  errorNombre, 
  errorAsunto, 
  errorMensaje ].forEach(e => e.innerText = '');
```

<hr>


[10]:https://javascript.info/ninja-code#brevity-is-the-soul-of-wit
[9]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND_assignment
[8]:./assets/utils/screenshot-3.png
[7]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet
[6]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions
[5]:./assets/utils/screenshot-2.png
[4]:./assets/utils/screenshot-1.png
[3]:https://stackoverflow.com/questions/3434278/do-dom-tree-elements-with-ids-become-global-properties
[2]:https://html.spec.whatwg.org/multipage/window-object.html#named-access-on-the-window-object
[1]:./assets/utils/screenshot-0.png
[0]:./assets/utils/presentacion.png


<!--TODO -->
<!--* /^[a-z]+\s[a-z]+$/ Nombre y apellido-->
<!--* para nombre /^[a-z]+[a-z ]+/gi -->
<!--* asunto y mensaje /^[a-z]+[a-z .]+/gim-->
<!--* /^[^ ][a-zñáéíóú ]+[a-zñáéíóú]+$/i -->