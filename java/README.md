# Java, POO y el Diseño Orientado a Objetos

El codigo minimo para un hola mundo:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hola mundo");
    }
}
```

En Java, cada archivo solo puede tener una sola clase publica,
en caso de ser publica, el nombre debe ser igual al nombre del
archivo.

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

<p style="text-align: center;">
<img src="java/img/simbologia.png"
     alt="Simbología de UML"
     style="background-color: #8ac;" />
</p>

En UML, puedo representar los atributos y de una clase si es lo
que me interesa. En cambio, si quiero representar las relaciones
entre diferentes clases pero sin importar su contenido, puedo
omitir sus atributos o metodos:

<p style="text-align: center;">
<img src="java/img/herencia.png"
     alt="Herencia en UML"
     style="background-color: #8ac;" />
</p>

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

Herencia ahora con una clase abstracta:

<p style="text-align: center;">
<img src="java/img/clase-abstracta.png"
     alt="Clases abstractas en UML"
     style="background-color: #8ac;" />
</p>

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

