# Homework JavaScript Avanzado I

## Scope & Hoisting

Determiná que será impreso en la consola, sin ejecutar el código.

> Investiga cuál es la diferencia entre declarar una variable con `var` y directamente asignarle un valor.

```javascript
x = 1;
var a = 5;
var b = 10;
var c = function (a, b, c) {
   var x = 10;
   
   console.log(x);
   console.log(a);
   var f = function (a, b, c) {
      b = a;
      console.log(b);
      b = c;
      var x = 5;
   };
   f(a, b, c);
   console.log(b);
};
c(8, 9, 10);
console.log(b);
console.log(x);
```

```javascript
console.log(bar);
console.log(baz);
foo();
function foo() {
   console.log('Hola!');
}
var bar = 1;
baz = 2;
```

```javascript
var instructor = 'Tony';
if (true) {
   var instructor = 'Franco';
}
console.log(instructor);
```

```javascript
var instructor = 'Tony';
console.log(instructor);
(function () {
   if (true) {
      var instructor = 'Franco';
      console.log(instructor);
   }
})();
console.log(instructor);
```

```javascript
var instructor = 'Tony';
let pm = 'Franco';
if (true) {
   var instructor = 'The Flash';
   let pm = 'Reverse Flash';
   console.log(instructor);
   console.log(pm);
}
console.log(instructor);
console.log(pm);
/*La diferencia entre declarar una variable con var y asignarle directamente un valor es cómo se maneja el alcance de la variable.

    Al usar var, la variable tiene un alcance limitado al ámbito de la función o al ámbito global si se declara fuera de una función.
    Al asignarle directamente un valor sin var, se crea una variable global si no existe una variable con el mismo nombre en un ámbito superior.

Es recomendable utilizar var, let o const para declarar variables de manera explícita y evitar la creación accidental de variables globales.*/
```

### Coerción de Datos

¿Cuál crees que será el resultado de la ejecución de estas operaciones?:

```javascript
6 / "3"
"2" * "3"
4 + 5 + "px"
"$" + 4 + 5
"4" - 2
"4px" - 2
7 / 0
{}[0]
parseInt("09")
5 && 2
2 && 5
5 || 0
0 || 5
[3]+[3]-[10]
3>2>1
[] == ![]
/*El resultado de la ejecución de las operaciones sería el siguiente:

javascript

6 / "3"       // Resultado: 2 (división de dos números)
"2" * "3"     // Resultado: 6 (multiplicación de dos números)
4 + 5 + "px"  // Resultado: "9px" (suma de números y concatenación con una cadena)
"$" + 4 + 5   // Resultado: "$45" (concatenación de cadenas y números)
"4" - 2       // Resultado: 2 (resta de un número como cadena y otro número)
"4px" - 2     // Resultado: NaN (la cadena no puede ser convertida a un número válido)
7 / 0         // Resultado: Infinity (división de un número por cero)
{}[0]         // Resultado: undefined (se intenta acceder a una propiedad inexistente en un objeto vacío)
parseInt("09")// Resultado: 9 (se interpreta la cadena como un entero en base 10)
5 && 2        // Resultado: 2 (operador lógico AND devuelve el segundo valor si ambos son verdaderos)
2 && 5        // Resultado: 5 (operador lógico AND devuelve el segundo valor si ambos son verdaderos)
5 || 0        // Resultado: 5 (operador lógico OR devuelve el primer valor verdadero)
0 || 5        // Resultado: 5 (operador lógico OR devuelve el primer valor verdadero)
[3]+[3]-[10]  // Resultado: 23 (se concatenan los arreglos en una cadena "33" y luego se restan 10)
3>2>1         // Resultado: false (las comparaciones se evalúan de izquierda a derecha)
[] == ![]     // Resultado: true (se produce una coerción de tipos y se compara un objeto vacío con un valor booleano negado)
*/
```

> Si te quedó alguna duda repasá con [este artículo](http://javascript.info/tutorial/object-conversion).

### Hoisting

¿Cuál es el output o salida en consola luego de ejecutar este código? Explicar por qué:

```javascript
function test() {
   console.log(a);
   console.log(foo());

   var a = 1;
   function foo() {
      return 2;
   }
}

test();

/*El output o salida en consola para el primer código sería el siguiente:

javascript

undefined
2 
La variable a se declara pero no se le asigna un valor antes de imprimir, por lo que su valor es undefined.
La función foo() devuelve 2 y se imprime en la consola*/

```

Y el de este código? :

```javascript
var snack = 'Meow Mix';

function getFood(food) {
   if (food) {
      var snack = 'Friskies';
      return snack;
   }
   return snack;
}

getFood(false);
/*undefined
    La variable snack se declara en el ámbito global y se le asigna el valor 'Meow Mix'.
    Dentro de la función getFood, se declara una variable local snack que se le asigna 'Friskies' si el argumento food es verdadero. Pero como el argumento es false, el valor de snack permanece indefinido.
    Se devuelve el valor de snack, que es undefined, y se imprime en la consola.
*/
```


### This

¿Cuál es el output o salida en consola luego de ejecutar esté código? Explicar por qué:

```javascript
var fullname = 'Juan Perez';
var obj = {
   fullname: 'Natalia Nerea',
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function () {
         return this.fullname;
      },
   },
};

console.log(obj.prop.getFullname());

var test = obj.prop.getFullname;

console.log(test());
/*El output o salida en consola al ejecutar este código sería el siguiente:
Aurelio De Rosa//
Juan Perez
la primera llamada a obj.prop.getFullname() imprime el valor 'Aurelio De Rosa', mientras que la segunda llamada a test() imprime el valor 'Juan Perez'. Esto se debe a las diferencias en el contexto (this) en el que se ejecutan las funciones.
*/

```

### Event loop

Considerando el siguiente código, ¿Cuál sería el orden en el que se muestra por consola? ¿Por qué?

```javascript
function printing() {
   console.log(1); //1
   setTimeout(function () {
      console.log(2);
   }, 1000); //4
   setTimeout(function () {
      console.log(3);
   }, 0); //3
   console.log(4);//2
}

printing();
El orden en el que se muestra por consola sería:

1
4
3
2

//Esto se debe a que los console.log(1) y console.log(4) se ejecutan inmediatamente. Luego, el console.log(3) se ejecuta debido a un setTimeout con un retraso de 0 milisegundos, pero se coloca en la cola de tareas y espera hasta que se complete el código principal. Finalmente, el console.log(2) se ejecuta después de un setTimeout con un retraso de 1000 milisegundos.
```

</br >

---

## **✅ FEEDBACK**

### Usa este [**formulario**](https://docs.google.com/forms/d/e/1FAIpQLSe1MybH_Y-xcp1RP0jKPLndLdJYg8cwyHkSb9MwSrEjoxyzWg/viewform) para reportar tus observaciones de mejora o errores. Tu feedback es muy importante para seguir mejorando el modelo educativo.
