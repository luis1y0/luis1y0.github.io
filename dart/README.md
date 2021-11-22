# El Lenguaje Dart

## Interfaces en el lenguaje Dart

En dart, cuando se declara una clase, esta implicitamente se puede
usar como una interfaz. Se le llama **Interfaces Implicitias**.

```dart
class Person {
  final String name;
  final int age;

  Person(this.name, this.age);

  bool isAdult() {
    return this.age >= 18;
  }
}
```

La clase anterior por ejemplo, se puede instanciar como o extender de ella
con la palabra reservada `extends`.

```dart
class Student extends Person {
  Student(String name, int age) : super(name, age);

  bool isAdult() {
    return super.isAdult();
  }
}
main() {
  var person = Person('Jhon', 24);
  var student = Student('Mary', 17);
  print(person.isAdult());
  print(student.isAdult());
}
```

En el ejemplo anterior, se puede usar perfectamente el constructor tanto de
Person como de Student para crear instancias, igualmente con extends se pueden
heredar las propiedades, comportamiento y el constructor de la clase base.

Como una clase se puede utilizar tambien como una interfaz para definir una API,
podemos "implementar" en vez de "extender" como en el ejemplo anterior.

```dart
class Teacher implements Person {
  final int id;
  // Al implementar Person como interface, no se heredan sus propiedades
  final int name;

  /// Cuando una clase se usa como interfaz, su constructor ya no existe, se toma
  /// como que la implementacion comienza en este punto
  Teacher({this.id, this.name});

  /// Solo la firma de los metodos se hereda, el comportamiento habitual
  /// de una interfaz que se usa para definir una API, en este caso, no se
  /// puede llamar a `super` para referirse a una super clase con una
  /// implementacion base, ya que al implementar se toma como que la primera
  /// implementacion comienza desde aqui
  bool isAdult() {
    return true;
  }
}
```

