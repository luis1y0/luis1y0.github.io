# CONVERSIÓN DE CODIGO A DIAGRAMAS DE CLASES

# Sobre las clases

Una clase es una plantilla que define la estructura con la que
se van a construir los objetos. La información almacenada dentro
de los campos del objeto suele denominarse **estado**, y
todos los métodos del objeto definen su **comportamiento**.

Una clase se compone de un conjunto de **atributos** y un 
**comportamiento** que lo generan los metodos definidos en ella.
Al hablar de una clase tambien se puede decir que define un
**tipo**, porque igual que int es el tipo de dato en la linea
`int age;` si existe la clase `class Person {}` se puede
declarar una variable de tipo Person con la linea
`Person personObject;` que es un tipo **no primitivo** al venir
de una clase, al ser un **objeto**; otros nombres para objeto
son **instancia** o **ejemplar de clase**.

## REPRESENTACIÓN DE UNA CLASE Y SU CONTENIDO

La forma de representar una clase es mediante un rectangulo dividido en 3
partes, esas partes son el nombre de la clase, sus atributos y su comportamiento
(metodos).

```java
class Person {
    private String name;
    private String lastName;
    private int age;
    private DateTime birthday;
    private String email;

    public Persona(String name, String lastName) {
        this.name = name;
        this.lastName = lastName;
    }

    public String getFullName() {
        return name + " " + lastName;
    }
    
    public void setAge(int age) throws Exception {
        if (age < 0) {
            throw Exception("Negative values not allowed");
        }
        this.age = age;
    }

    public boolean isAdult() {
        return age >= 18;
    }
}
```

El codigo anterior, se traduce a la siguiente figura:

<p style="text-align: center;">
<img src="java/img/simbologia.png"
     alt="Simbología de UML"
     style="background-color: #8ac;" />
</p>

Cada atributo y metodo lleva un nombre en el que se indica el su tipo despues
de dos puntos, tambien se escribe al principio un +/- para indicar si su
visibilidad es publica o privada. Los 3 puntos suspensivos los podemos usar
para indicar que hemos omitido otros atributos o metodos que no son importantes
para el contexto en el que nos encontramos.

En UML, puedo representar los atributos y de una clase si es lo
que me interesa. En cambio, si quiero representar las relaciones
entre diferentes clases pero sin importar su contenido, puedo
omitir sus atributos o metodos:

<p style="text-align: center;">
<img src="java/img/herencia.png"
     alt="Herencia en UML"
     style="background-color: #8ac;" />
</p>

```java
class Organism {}

class Plant extends Organism {}

class Animal extends Organism {}

class Cat extends Animal {}

class Dog extends Animal {}
```

En el siguiente diagrama, hay una flecha simple de **AirPort**
hacia la interface, **FlyingTransport**, que significa que el
aeropuerto usa un objeto con el tipo **FlyingTransport**, ese
tipo puede ser cualquiera de las 3 clases de abajo que implementan
esa interface. Implementar una interface se señala con una flecha
punteada con punta triangular hueca que va desde la
**clase concreta** hacia la **interface**.

<p style="text-align: center;">
<img src="java/img/interfaces.png"
     alt="Interfaces en UML"
     style="background-color: #8ac;" />
</p>

```java
class Airport {
    public void accept(FlyingTransport vehicle) {
        vehicle.fly("Mexico", "USA", 58);
    }
}

interface FlyingTransport {
    void fly(String origin, String destination, int passengers);
}

class Helicopter implements FlyingTransport {
    @override
    public void fly(String origin, String destination, int passengers) {}
}

class Airplane implements FlyingTransport {
    @override
    public void fly(String origin, String destination, int passengers) {}
}

class DomesticatedGryphon implements FlyingTransport {
    @override
    public void fly(String origin, String destination, int passengers) {}
}
```

Herencia ahora con una clase abstracta:

<p style="text-align: center;">
<img src="java/img/clase-abstracta.png"
     alt="Clases abstractas en UML"
     style="background-color: #8ac;" />
</p>

```java
abstract class Animal {
    public void makeSound();
}

class Cat extends Animal {
    public void makeSound() {}
}

class Dog extends Animal {
    public void makeSound() {}
}
```

Herencia ahora con una clase abstracta:

<p style="text-align: center;">
<img src="java/img/dependencia-asociacion.png"
     alt="Representacion de dependencia y de asociacion UML"
     style="background-color: #8ac;" />
</p>

Cuando un objeto de la clase Curso se usa en el
**ambito local** dentro de un metodo y un cambio en la firma
de sus metodos afecta al codigo de la clase Profesor, eso es
una dependencia. Cuando se usa de forma global en toda la clase
y sus metodos, se convierte en una asociación.

```java
class Professor {
    private Student student;
    // ...
    public void teach(Course c) {
        // ...
        this.student.remember(c.getKnowledge());
    }
}
```

Tipos de relaciones entre objetos:

<p style="text-align: center;">
<img src="java/img/tipos-relacion-poo.png"
     alt="Representacion de dependencia y de asociacion UML"
     style="background-color: #8ac;" />
</p>

