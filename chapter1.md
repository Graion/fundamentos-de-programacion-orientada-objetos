# Conceptos Básicos

Comenzemos este camino interminable de aprendizaje con los conceptos básicos de POO \(Programación Orientada a Objetos\) y a medida que avanzemos vamos a ir tocando temas más complejos.

## Objetos

¿Qué es un objeto?

La definición más básica dice: "Es un ente computacional que puede contener datos y comportamientos".

EJ: un usuario, una persona o un número entero. Cada uno de estos objetos tienen un comportamiento propio.

EJ: una persona puede comer/caminar/programar/atacar. Por lo tanto, un objeto lo podemos definir formalmente como: _Ente computacional que exhibe un comportamiento_.

## Mensaje

Todos los objetos de los cuales hablamos tienen comportamientos y se relacionan con otros objetos \(se piden cosas entre ellos\). Por cual podemos definir a un mensaje como:

_Interacción entre dos objetos: un emisor E y un receptor R. Un emisor le envía un mensaje a un receptor. El emisor puede obtener o no una respuesta._

Ej: caminar, pelear, comer, atacar.

## Ciclo de vida de un objeto

Todos los objetos tienen una vida, se puede decir que nacen cuando son instanciados y mueren cuando se los elimina de la memoria.

No obstante, existen dos formas de eliminar de memoria a un objeto dependiendo del lenguaje de programación. Los mecanismos se llaman Garbage Collector y los Destructores.

**Garbage Collector**: es un mecanismo que se encarga de borrar de la memoria las referencias a objetos y entidades que ya no se usan más, de manera que se pueda maximizar el uso del espacio en memoria.

**Destructores**: son métodos que se definen para cada objeto y cuyo principal objetivo es liberar los recursos que fueron adquiridos por el objeto a lo largo de su ciclo de vida y romper vínculos con otras entidades que puedan tener referencias a él.

## Métodos

¿Que es un método? ¿Qué diferencia existe con un mensaje?

_Un método es la sección de código que se ejecuta al enviar un mensaje._  Se identifica con una firma, que es la misma firma del mensaje enviado. Entonces, cuando un objeto recibe un mensaje, se ejecuta un método cuya firma es la misma que la del mensaje.

Ejemplo:

```text
method comer(Tarta tarta){
    calorias += tarta.calorias
}  
```

La firma de un objeto se define con tres componentes: 1. El nombre del método. 2. Los parámetros que recibe el método. 3. Lo que devuelve el método \(que puede ser nada u otro objeto\).

## Clases

Muchos lectores se preguntarán porque no empezamos con la definición de Clase. Esto se debe a que **las clases solo son una forma de implementar objetos**, pero no son la única manera de hacerlo \(como veremos más adelante\), por lo que fundamental que no pensemos automaticamente en clases cuando hablamos de objetos. Sin embargo, la mayor parte de los lenguajes de programación que usamos laboral y académicamente usan clases. Por eso vamos a tratarlas en este libro.

¿Qué es una clase? Buena pregunta.

Podemos definir a una clase utilizando dos definiciones complementarias: **Una clase es un molde, a partir del cual se crean los objetos**. Cuando instanciamos un objeto, el ambiente le pregunta a dicha clase que características y métodos tiene que tener el objeto que vamos a instanciar.

La otra definición es: **Una clase es un ente que determina el comportamiento y el tipo al que pertenecen sus instancias**.

**Ejemplo de código:**

```text
class Persona {
    public attribute calorias
    public method comer(Tarta tarta){
        self.calorias += tarta.calorias
    }
}

class Tarta{
    public attribute calorias
}

class ComidasTests{
    method ComerTartaTest (){
        var caloriasDespuesDeAlmorzar = 600
        Persona alan = new Persona()
        alan.calorias = 500
        Tarta tartaJamonYQueso = new Tarta()
        tartaJamonYQueso.calorias = 100
        alan.comer(tartaJamonYQueso)
        Assert.AreEqual(caloriasDespuesDeAlmorzar,alan.calorias)
    }
}
```

## Prototipos

La orientación a objetos basada en prototipos es un estilo de reutilización de comportamiento \(herencia\*\) que se logra por medio de la clonación de un objeto ya existente, que sirve como prototipo. Javascript, es el lenguaje orientado a objetos basado en prototipos más conocido.

## Autoreferencia

Así como un objeto puede conocer a otro objeto teniendo una referencia hacia este, también se puede conocer a sí mismo. Cualquier objeto tiene una auto-referencia, denominada this o self \(según el lenguaje\) para poder mandarse algún mensaje a si mismo.

Así como el self, hay otra referencia, denominada super o parent la cual es equivalente al this o self, con la particularidad de que le dice al Method LookUp “empezá desde 1 más arriba”. Es decir, que no busca la implementación en la clase donde está ejecutando el método, sino en la inmediata superior. \(puede que eso desemboque en que siga subiendo niveles\) En el 99% de los casos, sólo deberíamos llamar a super/parent para ejecutar el mismo método que estamos ejecutando \(En algunos casos, como Ruby, el lenguaje mismo nos lo restringe\). Es decir:

```text
class Gato {
    method caminar(){
        super.caminar() //No debería hacer, por ejemplo, super moverPiernaDerecha()
        self.lamerse()
    }
}
```

