#Hositing

**Hoisting** (literalmente, *izado*), es la acción por la cual el motor de Javascript "mueve hacia arriba", las declaraciones de variables y funciones en el código. Esto altera la idea de ejecución en orden estricto del código de Javascript, permitiendo la ejecución exitosa de casos como el siguiente:

```js
  a = 2;
  console.log(a); //2
  var a;
```

Lo que en realidad ocurre no es una alteración en el orden del codigo *per se* (lo cual tendría resultados caóticos en un programa), sino el resultado de la existencia de 2 fases separadas en la interpretación del código:

- Primero el código es **compilado**. En esta fase, entre otras cosas, se encuentran y asignan todas las declaraciones a su ámbito correspondiente.
- Luego el código se **ejecuta**, y es sólo en este momento en que las asignaciones, ejecución de funciones y cualquier otra operación sobre los datos ocurre.

Con lo cual lo que ocurre es que las sentencias declarativas (variables y funciones) existen en el programa primero; por ello se da el fenómeno del hoisting. Teniendo en cuenta la existencia de estas 2 fases, el código anterior es interpretado por el motor en las siguientes fases;

**Compilación**
``` js
  var a;
```

**Ejecución**
```js
  a = 2;
  console.log(a); //2
```

Una particularidad importante del hoisting es la siguiente: **las declaraciones de funciones son reconocidas antes que las de las variables**. Lo cual significa que, de tener declarados con el mismo nombre una función y una variable (a la cual podemos asignarle una función, aunque eso ocurrirá más tarde, durante la ejecución), la función será reconocida en primer lugar.
