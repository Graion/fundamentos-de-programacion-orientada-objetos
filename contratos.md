# Contratos

## Diseño por contratos

El diseño por contratos asume que todos los componentes del cliente que invocan una operación en un componente del servidor van a encontrar las precondiciones \(y postcondiciones\) especificadas como obligatorias para esa operación.

Muchas veces podemos tener algunas cosas para validar en nuestro sistema. Esto es:

* Pre-condiciones
* Post-condiciones
* Condiciones “permanentes” o invariantes

Por ejemplo, yo siempre que alimente a mi mascota, debo garantizarme que no esté llena. Por lo que quisiera tener algo como:

```text
public class Mascota implements Domesticable {
    public method alimentarse(){
        requires tengoHambre()
        //ejecutar método
    }
}
```

Lo mismo puede suceder con las post condiciones, o condiciones que se deben dar en todo momento.

```text
public class Mascota implements Domesticable{
    method alimentarse(){
        requires tengoHambre()
        //ejecutar método
        guarantees estoyLleno()
    }
}
```

Para hacer cumplir estos contratos, hay 2 maneras: 1. Validación manual y lanzamiento de excepciónes - Custom y fácil 2. Integrado por el lenguaje \(Eiffel\) o algún framework \(para PHP, Java, Ruby, etc\) - Beneficios en cuanto a meta-data

Lo importante es que se entienda el concepto del contrato, de ver que yo para poder interactuar de cierta manera con un componente, debo cumplir ciertos requisitos, tanto antes, durante o después de la interacción.

## Contratos & Herencia

Básicamente, aplican las mismas ideas y conceptos que en la herencia de comportamiento. Las precondiciones, postcondiciones e invariantes se heredan de clase a subclase.

Sin embargo existen algunos condicionamientos, para garantizar el principio de intercambiabilidad. Que se pueden resumir en la siguiente frase:

**Require no more, and promise no less**

Muy relacionado con la L \(Liskov Substitution Principle\) de SOLID, la cual plantea que donde uso una clase, debería poder usar cualquier subclase de ella y que siga funcionando todo correctamente.

