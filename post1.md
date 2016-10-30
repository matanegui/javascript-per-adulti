#Tipado en Javascript

Las variables en Javascript pueden tomar distintos valores de distinta naturaleza. En función a la naturaleza de dichos valores, esos valores pueden pertenecer a alguno de los siguientes **tipos**:

- `boolean` (true, false)
- `number` (cualquier valor numérico, entero o decimal, NaN, Infinity y otras constantes de Math)
- `string` (cualquier valor entre comillas simples o dobles, incluyendo la cadena vacía)
- `undefined` (cualquier variable no declarada)
- `null` (eL valor `null`)
- `symbol` (**nuevo en ES6**, constantes no asignadas o inicializadas con el constructor Symbol())
- `object` (cualquier otra cosa, incluyendo arrays)

De estos tipos, los primeros 6 son **primitivos**, es decir, son representaciones básicas de datos y no poseen métodos. Sólo las variables de tipo `object` admiten la definición de métodos.

Se puede obtener el tipo de una variable usando la función `typeof (variable)` (o `typeof variable`). Esta retorna un `string` que especifica el tipo de la variable pasada como parámetro; considerando esto, es recomendable que al chequear el tipo usando esta sentencia, se haga una comparación utilizando el operador === y el tipo contra el que se desea chequear en formato string.

``` js
// Demostración del formato recomendable para una comparación usando typeofs
if (typeof("Hola mundo!")==='string'){
  console.log("Este será el valor impreso!")
}
else{
  console.log("Nunca se imprimirá esto.")
}
```

Los tipos listados al principio refieren a los posibles valores que puede tomar una variable. Sin embargo, el uso de ``typeof`` para determinar el tipo de las variables no siempre producen el resultado esperado en función a lo establecido en esta lista. De hecho, los posibles resultados de la ejecución de `typeof` sobre cualquier variable son los siguientes:

- `boolean` (true, false)
- `number` (cualquier valor numérico, entero o decimal, NaN, Infinity y otras constantes de Math)
- `string` (cualquier valor entre comillas simples o dobles, incluyendo la cadena vacía)
- `undefined` (cualquier variable no declarada, **incluyendo null**)
- `symbol` (constantes no asignadas o inicializadas con el constructor Symbol())
- `function` (funciones)
- `object` (cualquier otra cosa, incluyendo arrays)
- Los `Host Objects`, objetos provistos por el entorno de ejecución de Javascript y no nativos del lenguaje, pueden producir otros resultados en esta función, cuyo contenido depende de dicho entorno.

¿Cómo se explica la diferencia entre la lista de tipos inicial y los posibles valores de retorno de esta función? A continuación, algunos casos dignos de análisis que justifican esta divergencia.

### Tipo de `null`

El valor `null` es de tipo `object`.

``` js
//Cual es el tipo de null?
console.log(typeof(null)); //object
```

Esto parece absurdo; lo esperable sería que `null` fuera de tipo `null`. El origen de esto radica en un [defecto en la primera implementación del lenguaje](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof). En general se lo considera como un error del lenguaje.

### Tipo de `undefined`

La palabra `undefined` no es una palabra reservada es Javascript. Esto implica que su nombre es tratado como cualquier otro nombre de identificador en Javascript: es, de hecho, una *propiedad del objeto global* que está inicializada con el valor undefined. Esto significa ques, inicialmente, el tipo de `undefined` es `undefined`, como el de cualquier variable no declarada. Sin embargo, esto puede cambiar si se le asigna un valor al identificador `undefined`.

``` js
//Cual es el tipo de undefined?
var tipoDeUndefined = typeof(undefined);
console.log(tipoDeUndefined); //undefined

//Cambiamos el valor...
var undefined = 4;
tipoDeUndefined = typeof(undefined);
console.log(tipoDeUndefined); //number
```

El uso de [modo estricto](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Modo_estricto) en Javascript permite que un error sea arrojado al intentar modificar esta variable.

### Tipo de `NaN`

Si bien este valor es inicializados como de tipo `number`, en todo lo demás el caso es idéntico a `undefined`, y todo lo dicho en el apartado anterior aplica de igual forma para `NaN`.

### Tipo de `Array`

Los arrays de Javascript son de tipo `object`. Esto es lógico, ya que, en efecto, las arreglos son objetos de Javascript, con una serie de propiedades y métodos particulares para su manipulación, para la construcción de los cuales se provee la sintaxis de corchetes como herramienta simplificadora. Esta notación no es sino una forma alternativa de notar a `Array`, el contructor utilizado para ellos, que es un objeto global de Javascript.

### Tipo de funciones

Si se aplica el operador `typeof` sobre una función, el tipo retornado será `function`. El único posible caso de confusión que puede presentarse respecto a esto es cuando le asignamos a una variable el resultado de la ejecución de una función (incluso si no contiene una sentencia `return`, pues las funciones retornan, por defecto, `undefined`). En ese caso, si se consulta el tipo, el resultado será el tipo del valor retornado por la función. Es decir, el único caso dudoso puede resultar de hacer `typeof` referenciando al resultado de invocar una función, en lugar de referenciar a la función propiamente dicha.

``` js
//Declaro una funcion
var funcion = function(){
  return "Soy una función";
};
console.log(typeof funcion); //function

//Asigno la funcion a una variable
var laMismaFuncion = funcion;
console.log(typeof laMismaFuncion); //function

//Asigno el resultado de ejecutar la funcion (retorna un string) a una variable
var laFuncionInvocada = funcion();
console.log(typeof laFuncionInvocada); //string
```
