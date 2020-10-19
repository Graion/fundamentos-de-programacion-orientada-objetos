# Polimorfismo

Es la capacidad que tiene un objeto de poder tratar indistintamente a otros que sean potencialmente distintos, es decir, es la capacidad que tienen distintos objetos de entender un mismo mensaje.

**Ejemplo**

Hay una persona, que siempre que llega a su casa interactúa con su mascota. Como su mascota es un perro, lo que hace es jugarle.

```text
class Persona {
    public attribute mascota
    public method llegarACasa(){
        mascota.ladrar()
    }
}

class Perro {
    public method ladrar(){
        ...
    }
}
```

¿Qué pasa si ahora si la persona vende al perro y se compra un gato? Debemos saber interactuar con ambos tipos de animales.

```text
class Persona {    
    //Método no polimórfico
    public method llegarACasa(){
        if(mascota isA Perro)
            mascota.ladrar()
        else if(mascota isA Gato)
            mascota.maullar()
    }

    //Método polimórfico
    public method llegarACasa(){
        mascota.saludar()
    }
}
```

Este concepto se conoce como **polimorfismo**. Es la capacidad de intercambiar un objeto con otro, abstrayendo al que lo conoce de su implementación.

### ¿Por qué es importante diseñar polimórficamente?

Para ganar extensibilidad. Sino, cada vez que aceptemos otro animal como mascota, vamos a tener que modificar la clase. \(Rompemos con la O de S.O.L.I.D.\) En cambio, si lo mantenemos, cuando agregue una mascota, es suficiente con crearle una implementación de estos métodos para que encaje perfectamente en el sistema.

Entonces, para hacerlo genérico, se establece un contrato: "Todas las mascotas deben entender el mensaje interactuar". Agregamos algo más, no queremos interactuar con nuestra mascota si está dormida, por lo que: "Todas las mascotas deben evitar interactuar si están dormidas".

En general, un contrato establece un acuerdo entre dos \(o más\) partes. Si lo cumplimos el sistema va a tener una funcionalidad dada, o un comportamiento. De alguna forma regula la interacción entre dos módulos de nuestra aplicación. Entonces intercambiando un módulo por otro que cumple el mismo contrato debería ser transparente para el otro módulo.

La parte del contrato más frecuentemente usada, va a ser lo que denominamos interfaz. La interfaz de un objeto es el conjunto de mensajes que entiende.

Adicionalmente, están las construcciones específicas denominadas interfaces. Estas, no son más que una especificación del conjunto de mensajes que tienen que cumplir aquellos que la implementen.

Va de nuevo, supongamos que todos los animales “Domesticables” son aquellos a los que se puede alimentar o se puede jugar con. Podríamos definir la interfaz Domesticable de la siguiente manera:

```text
interface Domesticable{
    method alimentarse()
    method jugar()
}
```

Nótese que no le definimos ningún comportamiento o implementación. Lo único que establecemos es “Todos aquellos que quieran ser domesticables, tendrán que poder alimentarse y jugar”.

Una buena thumb rule para identificar la necesidad de una interfaz es establecer un adjetivo común a los objetos que quiero que la implementen \(Domesticable/Amable/Convertible\), a diferencia de una superclase \(veremos más adelante\), las cuales suelen ser sustantivos \(Animal/Persona/Auto\)

## Clase Abstracta

Cuando vimos lo de las pre-condiciones, apareció la clase Mascota..

```text
class Mascota implements Domesticable{
    method alimentarse(){
        requires noEstoyLleno()
        //ejecutar método
    }
}
```

Pero, ¿Va a haber algún objeto Mascota o todos van a ser Perros/Gatos/Iguanas/etc?

En este caso, la clase Mascota no debería ser instanciable, solo quiero utilizarla para que las clases que heredan de esta puedan usar el código definida en ella.

A esto se lo llama una **clase abstracta**. Al contrario de las abstractas, las clases que sí son instanciables, se las denomina concretas.

Una clase abstracta define sólo la interfaz o firma de algunos de sus métodos. Podemos tener:

* clases abstractas puras \(¿interfaces\*?\).
* clases parcialmente abstractas.

Manteniendo el concepto, una clase abstracta puede obligar a que sus subclases implementen cierto método, pero sin definir ningún comportamiento por default. A estos métodos, naturalmente, se los denomina métodos abstractos.

Si una clase tiene al menos UN método abstracto, entonces debe ser abstracta, porque no tiene sentido que haya un objeto que sea instancia de ella.

A fines de comprensión de concepto, podría decirse que una interfaz es una clase abstracta con todos sus métodos abstractos.

## Interfaces

Una interfaz define un **tipo** y es una colección de **métodos abstractos**. De forma similar a una clase abstracta, obliga a quienes implementen la interfaz a implementar los métodos. Las interfaces no pueden definir la implementación de los métodos de los objetos que las van a utilizar, pero si obligan a dichos objetos a definir esa implementación.

Las interfaces sirven para solventar la limitación de muchos lenguajes, que no soportan la herencia múltiple.

## Clases abstractas puras vs interfaces

* Solo se puede extender de una clase abstracta, pero se pueden implementar más de una interfaz en una clase.
* Un interfaz no puede definir atributos.
* Así como no se puede crear instancias de clases abstractas, tampoco es posible crear instancias de interfaces.

