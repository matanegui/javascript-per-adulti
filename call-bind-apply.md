# Call, bind y apply

Son tres métodos que forman parte del prototipo del objeto *Function*. Como tales, pueden ser invocados como métodos de cualquier función que se declare. De aquí en mas, la función sobre la cual se llame cualquiera de estos métodos será llamada **función destino**.

## Bind

El método ```Function.bind(thisArg, arg1, arg2...)``` permite pasar como primer argumento un objeto: este objeto será el que, dentro de la función destino, será referenciado cuando usemos la palabra reservada ```this```. El resto de los valores pasados son argumentos que serán pasados a la función destino, aparte de aquellos que especifiquemos explícitamente al hacer el llamado a dicha función.

``` js
(function bindTest(){
  var person = {
    "firstName" : "Carlos",
    "sex" : "Male"
  }

  function greet(){
    console.log("Hello, "+ this.firstName + "!");
  }

  greet(); //Hello, undefined!
  greet.bind(person)(); // Hello, Carlos!
})();
```

Cabe aclarar que ```apply``` **no invoca** a la función destino, sino que nos devuelve una referencia a la función con el contexto especificado en los parámetros; para invocarla habrá que hacerlo específicamente.

Resulta especialmente útil para asegurar que la palabra reservada ```this``` está ligada al objeto que deseamos referenciar dentro de la función, es decir, que siempre apuntará a dicho objeto. Esto es especialmente útil cuando se manejan callbacks. Además, esto nos permite tomar prestados métodos de ciertos objetos y aplicarlos a otros objetos diferentes.

También permite hacer **currying**: dividir una función con varios parámetros en varias funciones que toman una parte de los parámetros cada una. Este es un concepto clave en la programación funcional; permite tener un conjunto de funciones parciales que a través de su composición llevan a cabo las operaciones deseadas y pudiendo ser reusadas para fines distintos.

Esto se logra empleando los parámetros que vienen después del ```thisArg```:

``` js
(function (){
  var person = {
    "firstName" : "Carlos",
    "sex" : "Male"
  }

  function greet(mood, weather){
    console.log("Hello, "+ this.firstName + "! Hope you're feeling "+mood+" on this "+weather+" day!");
  }

  //Siempre queremos que el saludo sea optimista en esta parte del programa!
  var happyGreet = greet.bind(person, "happy");

  happyGreet("rainy"); //Hello, Carlos! Hope you're feeling happy on this rainy day!
})();
```

## Call y apply

Similares objetivos pueden alcanzarse empleando ```call``` y ```apply```. La diferencia con ```bind``` reside en que, en ambos casos, la función referida **sí es invocada** al utilizar estos métodos. La diferencia entre ambas es la forma de pasaje de los parámetros.

### Call

El método ```Function.call(thisArg, arg1, arg2...)```, al igual que ```bind```, recibe como primer parámetro el objeto que será referenciado por ```this```, y luego los parámetros que serán pasados a la función referida en su ejecución, en orden.

Puede reescribirse el anterior ejemplo usando ```call```, de la siguiente forma:

``` js
(function (){
  var person = {
    "firstName" : "Carlos",
    "sex" : "Male"
  }

  function greet(mood, weather){
    console.log("Hello, "+ this.firstName + "! Hope you're feeling "+mood+" on this "+weather+" day!");
  }

  //Siempre queremos que el saludo sea optimista en esta parte del programa!
  greet.call(person, "happy", "rainy");
})();
```

Como puede verse, la diferencia principal radica en que se hará una invocación directa a la función referida, y que, en este caso, será preciso pasar todos los parámetros cuando se haga la llamada a ```call```.

### Apply

De forma similar funciona el método ```Function.apply(thisArg, [arg1, arg2...])```. El primer parámetro también indica el valor de ```this``` en el contexto de la función. La diferencia es que los parámetros de la función referida son provistos, en orden, utilizando un array.

Nuevamente puede reescribirse nuestro ejemplo empleando apply:

``` js
(function (){
  var person = {
    "firstName" : "Carlos",
    "sex" : "Male"
  }

  function greet(mood, weather){
    console.log("Hello, "+ this.firstName + "! Hope you're feeling "+mood+" on this "+weather+" day!");
  }

  //Siempre queremos que el saludo sea optimista en esta parte del programa!
  var greetParams =  ["happy", "rainy"];
  greet.apply(person, greetParams);
})();
```
