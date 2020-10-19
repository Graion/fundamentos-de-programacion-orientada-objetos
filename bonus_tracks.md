# Bonus Tracks

## Bonus Tracks

Los siguientes temas, ya no están tan relacionados con POO, pero también nos parecieron interesantes y decidimos mencionarlos brevemente para que el lector se lleve un poco más.

### Excepciones

¿Cuántas veces recibimos un mail de un cliente diciendo que apareció una pantalla con un mensaje de error poco descriptivo?

Esto probablemente se deba a un bajo nivel de detalle en el manejo de excepciones dentro de nuestra aplicación. El manejo de excepciones consiste en controlar los errores que surjan dentro de nuestro sistema para poder tratarlos debidamente.

Tratarlos debidamente implica:

* Mostrar un mensaje más amigable a los usuarios \(la más común\).
* Hacer un rollback de alguna transacción.
* Escribir en un log el error para analizarlo.
* Si el error no afecta el flujo de trabajo: contenerlo para que el flujo siga.

Y así podemos seguir durante un buen rato...

**Ejemplo básico**:

```text
public method GuardarInformacionImportante(Factura factura) {
    try {
        // Se ejecuta algo que puede producir una excepción
        // Un error en la base, la famosa división por cero, una referencia a un null, etc.
    } catch (DividedByZeroException e) {
         // manejo de una excepción de una división por cero
    } catch (Exception e) {
         // manejo de una excepción cualquiera
    } finally {
         // código a ejecutar haya o no excepción
    }
}
```

La mayoría de los lenguajes de programación soportan estos tipos de sentencias. Vale la pena resaltar la línea “catch \(Exception e\)”, esta sentencia es casi obligatoria, ya que su función es atrapar cualquier tipo de excepción que no allá sido atrapada previamente. En nuestro ejemplo anterior solamente “atrapamos” las excepciones que sean del tipo “división por cero”, si dentro de la sección try se hubiese lanzado otro tipo de excepción como un error de conexión a la base de datos, este error habría sido atrapado en la sección de excepciones genéricas.

**Forzando una excepción**

Muchas veces existen ocasiones donde existen errores que si bien no hacen que nuestra aplicación explote por los aires, si afectan nuestro flujo de negocios, es por eso que muchos lenguajes permiten lanzar excepciones manualmente y nuestro programa las va a ver como si fueran excepciones comunes. Ejemplo:

```text
public method void IrATrabajar(Dia dia, Trabajador trabajador) {
    try {
        if(dia.EsLunes() && trabajador.SalioElFinde())
            throw new Exception()
    } catch (Exception e) {
         Interface.MostrarError(“El trabajador está enfermo”);
    } finally {
         // código a ejecutar haya o no excepción
    }
}
```

**Creando nuestras propias excepciones**

Así como podemos forzar excepciones, también podemos crear nuestros propios tipos de excepciones, conocidas como “User-Defined-Exceptions”. ¿Para que? Para poder definir comportamientos propios de excepciones dentro de nuestro dominio. Es decir, supongamos que estamos haciendo un método que registre a personas físicas en nuestro sistema, pero que no soporte personas jurídicas!

Primero definimos nuestro tipo de excepción:

```text
public clase EsPersonaJuridicaException: Exception
{
    public EsPersonaJuridicaException()
    {
    }
}
```

Luego la usamos:

```text
public metodo RegistrarPersona(Persona persona) {
    try {
        if(persona.EsJuridica())
        throw new EsPersonaJuridicaException()
    } catch (EsPersonaJuridicaException e) {
         Interface.MostrarError(“No se pueden registrar personas jurídicas”)
    }catch (Exception e) {
         Interface.MostrarError(e.Message)
    } finally {
         // código a ejecutar haya o no excepción
    }
}
```

En este caso parece medio trivial definir un tipo excepción para algo tan trivial, pero cuando el tipo de excepción se da en varios casos y tiene un comportamiento más elaborado, se justifica crear nuestro propio tipo de excepción.

Un detalle fundamental que no debemos olvidar, es que nuestro tipo de excepciones deben heredar de la clase Exception, ya para poder utilizar la sentencia try nuestras Excepciones deben heredar las propiedades de una excepción básica.

### Colecciones

A veces necesitamos tener algo que no es un solo objeto, sino que en sí mismo contenga 0, 1 o varios objetos. Estos “elementos”, los vamos a llamar genéricamente Colecciones y también son objetos.

Eso significa que como cualquier otro, va a poder recibir mensajes y hasta le podríamos generar nuevos métodos para que entiendan \(peligroso\) y mejor aún, crear nuestro propio tipo de Colección que cumpla con lo que queramos.

Cabe destacar, que en los lenguajes tipados, todos los elementos de una colección deben ser del mismo tipo, es decir, deben \(al menos\) implementar la misma interfaz. Genéricamente, los elementos de una colección deben ser polimórficos, según lo que quiera hacer con ellos.

La ventaja de usar los métodos de colecciones por sobre la iteración por elemento, es ganar en declaratividad y expresividad, dejando un código más fácil de entender, modificar y escalar.

**¿Qué cosas podemos querer hacer con una colección?**

* Agregarle elementos.
* Saber el tamaño.
* Hacer un filtro o selección de los elementos según un criterio.
* Recorrerla y hacer algo con cada elemento.
* Preguntarle si contiene o no un determinado objeto.
* Ordenarla.

#### Agregar elementos

Este método es bastante trivial. Si bien depende del lenguaje, suele haber un método push/ add o similares.

#### Saber el tamaño

También es algo bastante trivial. El método suele ser size o length. Un caso de uso bastante común es saber si una lista está vacía. Hay algunos lenguajes que tienen el mensaje isEmpty o similar, y sino, simplemente hacer if\(!colec.length\).

#### Filtrar

Acá empieza la ventaja de usar los métodos de las colecciones frente al paradigma imperativo. ¿Qué podríamos hacer de forma imperativa si queremos filtrar elementos según una condición?

```text
public method algunFiltrado(){
   t_objeto colecaux[];
   for (int i = 0; i <= colec.length; i++) {
      if(colec[i].edad >= 18) colecaux[] = colec[i];
   }
   return colecaux;
}
```

Como vimos antes, esto tiene muchas bajas en cuanto a expresividad y declaratividad.

Por esta razón, las colecciones nos suelen proveer un método filter o similar que recibe una condición y devuelve los elementos de esa colección que la cumplan. Si bien se puede filtrar por un campo, también podría filtrarse por un método o un conjunto de condiciones que devuelvan un booleano.

El mismo caso de uso, utilizando el método filter, se vería de la siguiente manera

```text
public method algunFiltrado(){
    return colec.filter(mayorDeEdad);
}

function mayorDeEdad(Persona persona){
    return persona.edad >= 18
}

public method algunFiltrado(){
    return colec.filter(\x -> x.edad >= 18); //Lambdas FTW
}
```

#### Recorrerla

Hay un método que denominamos genéricamente \(y por ser usado por la mayoría de los lenguajes\): map

Es un método que entienden las colecciones y que también recibe como parámetro un ejecutable. Léase bloque para Smalltalk/Ruby, función para JS, callable para php, etc. Este ejecutable, va a recibir un elemento de la colección, y puede o no retornar otro elemento. Devuelve otra colección con los cambios correspondientes según lo que se ejecute en el parámetro. \(si no se devuelve nada, devuelve la misma colección\)

Ejemplo de uso en JS:

```text
[1,2,3,4].map(function(num) {
    return num*2;
});
```

Desde adentro de la función que recibe el map, uno podría querer cambiar alguna variable externa. Si bien en muchos lenguajes se permite, hay que tener cuidado con el scope.

Saber si un elemento pertenece a una colección Este método es muy similar al filter, pero en lugar de devolver una colección con los elementos que cumplen la condicíon dada, devuelve solo uno. Suele denominarse find.

#### Ordenarla

El método \(genéricamente denominado sort\) tiene como objetivo devolver una colección ordenada por un criterio. También es de orden superior, y en general recibe una función con 2 argumentos, los cuales representan a 2 elementos de la colección. La función debe determinar la condición para saber si una va antes que la otra \(ya sea con un booleano o si un número es mayor o menor a 0\).

```text
products.sort( function(a, b) { 
   return a.price - b.price; //ascending order
});

products.sort( function(a, b) { 
   return a.price > b.price; //ascending order
});

products.sort(‘price’); //solo para atributos
```

Como mencionamos antes, la mayor potencia de todos estos métodos, además de su expresividad y declaratividad, es la facilidad de encadenarlos.

```text
public method montañaRusa(){
    return colec
    .filter(midenMasDe(1,50))
    .sort(‘llegada’)
    .map(function(jugador){ jugador.sacudir() });
}
```

## Variables y métodos de clase

También se denominan métodos y variables estáticas. La implementación de los mismos varía dependiendo del lenguaje. En algunos \(como Ruby o Smalltalk\) las clases son objetos, en otros \(como Java o PHP\), las clases son entes particulares con una instancia asociada, que se pueden utilizar de determinada forma y enviarles mensajes y en algunos \(JavaScript\) no se utiliza ninguna de estas nociones, pero siguen teniendo el mismo concepto.

Las variables de clase nos sirven cuando queremos que nuestros objetos tengan alguna referencia a algún valor, que sea el mismo para todas las instancias de esa clase, y que además pueda cambiar \(por eso usamos una variable y no hardcodeamos ese valor en el código del programa, asumiendo que es posible\).

Si usáramos una variable de instancia con la intención de settear el mismo valor a todas las instancias de esa clase, y luego queremos cambiar el valor de modo de afectar a todas las instancias, tengo que poder encontrar todas las instancias ya existentes para poder mandarles el mensaje para que actualicen su referencia... Y si tengo muchos muchos objetos eso no está bueno, no sólo por la complejidad innecesaria del problema, sino también porque la performance puede verse afectada

Los métodos de clase, son métodos cuyo receptor / implementador va a ser una clase en lugar de un objeto. El caso más común es el del constructor, donde la clase recibe el “new” y ella sabe generar una nueva instancia e incluso llamar al constructor correspondiente.

Otro caso recurrente es el de querer tener una sola instancia por clase. Para profundizar más en este funcionamiento, ver el patrón de diseño Singleton.

