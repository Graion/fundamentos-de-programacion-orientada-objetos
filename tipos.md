# Tipos

¿Qué es un tipo? Un tipo describe un conjunto de valores.

La idea de tipo nos permite relacionar:

* un conjunto de valores que tienen ese tipo o son de ese tipo,
* con las operaciones que pueden ser realizadas sobre esos valores.

Los objetivos de un sistema de tipos son:

* Ayudar a detectar errores al programar.
* Guiar al programador sobre las operaciones válidas en un determinado contexto, tanto en cuanto a documentación como en cuanto a ayudas automáticas que puede proveer por ejemplo un IDE.
* En algunos casos el comportamiento de una operación puede variar en función del tipo de los elementos involucrados en la misma. Polimorfismo, sobrecarga \(veremos más adelante\), multimethods, etc.

Podemos hacer 3 clasificaciones de tipado:

* Implícito o explícito. Un lenguaje va a tener tipado explícito si:
  * Todos los elementos \(variables, métodos, etc\) tienen un tipo definido.
  * Para que dos objetos sean polimórficos, debo explicitarlo \(por una interfaz o por heredar de la misma clase\).
* Chequeo dinámico o estático.
  * La diferencia está en el momento en que es ejecutado el chequeo de tipos. En los lenguajes de chequeo estático \(como Java o C\#\) se chequea en tiempo de compilación, mientras que en los de chequeo dinámico \(como Ruby, PHP o JS\) se realiza en tiempo de ejecución.
  * El chequeo dinámico da lugar a un concepto famoso denominado Duck Typing. El cual se basa en que “si algo tiene pico de pato, camina como pato, y hace cuack, es un pato”. Es decir, toma sentido el tipo al que pertenece un objeto según qué mensaje puedo mandarle y cómo responde, no se me especifica desde antes.
* Estructural o nominal
  * No nos vamos a detener mucho en este. Hace referencia a si se identifica un tipo por su nombre o por su estructura. Es muy común en los lenguajes del paradigma funcional \(otro paradigma\).

Combinaciones más frecuentes:

* Explícito, estático y nominal
* Implícito y dinámico

## Casteo

Es un mecanismo utilizado en los lenguajes de chequeo estático, por el cual nosotros le “aseguramos” al compilador que cierto objeto pertenece al tipo especificado.

Ejemplo:

```text
//Suponemos que los métodos de la API siempre reciben un Object
method recibirPerroDeApi(Object obj){
    var perro = (Perro) obj
    perro.ladrar()
}
```

Hay que ser muy cuidadosos al usar el casteo, porque podría llevar a “confundir” al compilador y hacer que rompa en tiempo de ejecución \(perdiendo el beneficio que nos da el chequeo estático\), por ejemplo si en Object que se recibe es un Gato en vez de un perro, al llamar al método "ladrar" nuestro programa lanzará una excepción.

