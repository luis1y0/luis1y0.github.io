# Controladores

## Estructura de un controlador

Un controlador (@Controller) se encarga de atender las
peticiones HTTP y es una versión derivada de un componente
(@Component), cuando una clase se marca como controlador, los metodos de esa
clase que manejan una request deben responder con el nombre de una vista a
renderizar. Un controlador REST (@RestController) es una versión derivada de un
controlador (@Controller), la diferencia es que la respuesta de sus metodos
manejadores son el cuerpo de la respuesta HTTP (@ResponseBody).

Cuando se usa la anotación @Controller, el tipo de retorno de los metodos
manejadores (method handlers) debe ser un String que contiene el nombre de la
vista renderizar, por defecto se usa `JSP`, pero si se agrega la dependencia de
`Thymeleaf`, el ejemplo siguiente lo que hace es tomar el string `"file-name"`
y buscar en la ruta `src/main/java/resources/templates/` un archivo llamado
`file-name.html`:

<!-- tabs:start -->

#### **ClassName.java**

src/main/java/package

```java
@Controller
@RequestMapping("/")
public class ClassName {
  @GetMapping("/index")
  public String getIndexPage() {
    return "file-name";
  }
}
```

#### **file-name.html**

src/main/resources/templates

```html
<html>
<body>
  <p>Pagina index</p>
</body>
</html>
```

<!-- tabs:end -->

Con el controlador anterior, esta seria la solicitud que un cliente realizaria
y la respuesta obtenida:

<!-- tabs:start -->

#### **Request**

http://localhost:8080/index

```
GET /index HTTP/1.1


```

#### **Response**



```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 51

<html>
<body>
  <p>Pagina index</p>
</body>
</html>
```

<!-- tabs:end -->

En el caso de usar @RestController, lo que regresan los metodos de la clase es
lo que directamente se transforma en el cuerpo de la respuesta HTTP, si el tipo
de dato es un primitivo, directamente se regresa su representación en forma de
texto, en caso de un objeto no primitivo, se realiza una transformación al tipo
de contenido que se configura a responder en una api, ya sea JSON o XML
tipicamente:

```java
@RestController
@RequestMapping
public class ClassName {

  @GetMapping("/users/length")
  public int getNumberOfUsers() {
    return 5;
  }
  
  @GetMapping("/users/{id}")
  public User getUserById(@PathVariable int id) {
    return Map.of(
      "id", id,
      "name", "Juan",
      "age", 18
    );
  }
  
  @GetMapping("/users")
  public ArrayList<User> getUsers() {
    return List.of(
      Map.of("id", 1, "name", "Juan", "age", 18),
      Map.of("id", 2, "name", "Maria", "age", 20),
      Map.of("id", 3, "name", "Pablo", "age", 25)
    );
  }
}
```

<!-- tabs:start -->

#### **/users/length**

http://localhost:8080/users/length

```Request
GET /users/length HTTP/1.1


```

```Response
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1

5
```

#### **/users/34**

http://localhost:8080/users/34

```Request
GET /users/34 HTTP/1.1


```

```Response
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 32

{"id":34,"name":"Juan","age":18}
```

#### **/users**

http://localhost:8080/users

```Request
GET /users HTTP/1.1


```

```Response
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 99

[{"id":1,"name":"Juan","age":18},{"id":2,"name":"Maria","age":20},{"id":3,"name":"Pablo","age":25}]
```

<!-- tabs:end -->

@RestController fue introducido en Spring 4.0 para 
simplificar la creacion de servicios web RESTful (es un anotacion
conveniente que combina @Controller y @ResponseBody, que elimina la necesidad
de anotar cada metodo de manejo de request con @ResponseBody).

Despues de clasificar a una clase con @RestController se debe indicar la ruta
base a partir de la cual todos los metodos de la clase usan, si no se
especifica, el valor es un string vacio que igual a la raiz de la ruta HTTP.

Las siguientes 2 clases generan el mismo resultado:

<!-- tabs:start -->

#### **Controller.java**

src/main/java/package

```java
@Controller
@RequestMapping("/api/v1")
public class Controller {
  @GetMapping("/result")
  public @ResponseBody Boolean getResult() {
    return true;
  }
}
```

#### **RestController.java**

src/main/java/package

```java
@RestController
@RequestMapping("/api/v1")
public class RestController {
  @GetMapping("/result")
  public Boolean getResult() {
    return true;
  }
}
```

<!-- tabs:end -->

## Estructura de una peticion HTTP

![Partes de una peticion HTTP](../images/http-diagram.png)

1. Los verbos HTTP se filtran a traves de las anotaciones que descienden de
 `@RequestMapping`. Como es `@GetMapping`, `@PostMapping`, `@PutMapping`,
 `@DeleteMapping` y `@PatchMapping`, una de estas anotaciones se agrega a un
 metodo, ese metodo es llamado **Method handler** (metodo manejador).

2. La ruta o endpoint se agrega como parametros de las anotaciones de tipo
 `@RequestMapping("<ruta o endpoint>")`.

3. El **Query String** se extrae en Spring a traves de los parametros del
 del method handler, por cada campo del query string, se agrega un parametro del
 tipo correspondiente y una anotación
 `@RequestParam(<nombre>, defaultValue=<valor>)` en la que se indica el nombre
 del parametro y un valor por defecto en caso de que el parametro este ausente
 en la url:
```java
  @GetMapping("/api/v1/books")
  public String getData(
     @RequestParam("pagina", defaultValue=1) int page,
     @RequestParam("por_pagina", defaultValue=10) int perPage
  ) {
     String formattedText = "Mostrando %d elementos de la pagina %d";
     return String.format(formattedText, perPage, page);
  }
```
Si se quiere obtener todos los parametros en una sola variable, se puede
declarar un parametro de tipo `Map<String, String>` y la anotacion
`@RequestParam` sin especificar un identificador o nombre de parametro.
```java
  @GetMapping("/api/v1/books")
  public String getData(@RequestParam Map<String, String> params) {
     String formattedText = "Mostrando %d elementos de la pagina %d";
     return String.format(formattedText, params.get("per_page"), params.get("page"));
  }
```

<!-- tabs:start -->

#### **Request**

http://localhost:8080/api/v1/books?pagina=1&por_pagina=10

```
GET /api/v1/books?pagina=1&por_pagina=10 HTTP/1.1


```

#### **Response**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 37

Mostrando 10 elementos de la pagina 1
```

<!-- tabs:end -->

4. Los headers pueden accesarse igual que los parametros del query string, se
 usa la anotación `@RequestHeader(<nombre>)` en un parametro de tipo String,
 si el tipo es diferente de String se aplica una conversión automatica. Si no se
 especifica un nombre de header y el tipo de dato del parametro del metodo
 manejador es `Map<String, String>`, entonces el parametro recibe todos los
 headers en ese parametro. Hay un tipo especial de Header que guarda una lista
 de cookies, se puede accesar a cada cookie con la anotación
 `@CookieValue(<nombre>)`.
```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Content-Type: application/json
Cookie: dark_mode=true; pagina=3; por_pagina=10
```

5. Para accesar al body de la siguiente petición, se debe anotar un parametro del **method handler** con la anotación
`@RequestBody` y spring leera el body lo deserializara dentro de un objeto
del tipo que se especifique en el parametro:
```java
  @PostMapping("/api/v1/ruta")
  public String getData(@RequestBody String body) {
     // la variable <body> tiene el valor "contenido del body"
     String formattedText = "Valor: %s";
     return String.format(formattedText, body);
  }
```

<!-- tabs:start -->

#### **Request**

http://localhost:8080/api/v1/ruta

```
POST /api/v1/ruta HTTP/1.1
Host: localhost:8080
Content-Type: text/plain
Content-Length: 18

contenido del body
```

#### **Response**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 25

Valor: contenido del body
```

<!-- tabs:end -->
Hay otro tipo de body usado especialmente cuando se envian archivos porque
permite combinar los campos de archivo con campos de datos simples como texto,
ese tipo de body se llama **multipart** y por ejemplo se puede generar con el
siguiente formulario HTML un multipart con un texto y un archivo en la misma
petición:
```html
<!DOCTYPE html>
<html>
<head><title>Formulario</title></head>
<body>
  <form
   method="POST"
   action="/api/v1/ruta"
   enctype="multipart/form-data">
    <input type="text" name="nombre">
    <input type="file" name="archivo">
    <input type="submit">
  </form>
</body>
</html>
```
El formulario anterior genera la siguiente peticion HTTP (simplificada):
```
POST /api/v1/ruta HTTP/1.1
Host: localhost:8080
Content-Length: 288
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryoqCCAZrc6DaOAXAN

------WebKitFormBoundaryoqCCAZrc6DaOAXAN
Content-Disposition: form-data; name="nombre"

luis
------WebKitFormBoundaryoqCCAZrc6DaOAXAN
Content-Disposition: form-data; name="archivo"; filename="documento.txt"
Content-Type: text/plain

contenido del archivo, solo una linea
------WebKitFormBoundaryoqCCAZrc6DaOAXAN--
```
Si el body de la peticion es **multipart**, se puede usar la
anotación `@RequestParam(<nombre de parte>)`, se usa igual que request body.
Si se quiere leer un body o una parte como archivo, se puede especificar la
interface `MultipartFile` como tipo del parametro del method handler:
```java
@PostMapping("/api/v1/ruta")
public String getData(@RequestParam("nombre") String name, @RequestParam("archivo") MultipartFile file) {
  String formattedText = "El nombre es %s, y el nombre del archivo es %s";
  return String.format(formattedText, name, file.getName());
}
```
Lo anterior devolvera una respuesta parecida a esta:
```
HTTP/1.1 200 OK
Content-Length: 59
Content-Type: text/plain

El nombre es luis, y el nombre del archivo es documento.txt
```
Si deseas acceder a la lista de todos los campos que sean alamacenados como
texto se puede usar la anotacion `@RequestParam` sin especitcar un nombre de
campo y usando el tipo de dato `Map<String, String>`. En cambio si se quiere
acceder a todos los archivos del formulario solo cambias el tipo de dato a
`Map<String, MultipartFile>` como tipo del parametro.

### Resumen de anotaciones

- RequestMapping: <Get|Post|Put|Delete>Mapping para cada verbo
- RequestParam: se usa para acceder a un Query String o campo de formulario
- RequestHeader y CookieValue: para acceder un header especifico o de cookie
- RequestBody: Para **parsear** completamente el contenido del body

## Mapeando metodos HTTP

En un controlador, la ruta y el metodo HTTP a escuchar se configura a tráves
de la anotación `@RequestMapping`, para mayor facilidad existen las siguientes
anotaciones, para los metodos HTTP más comunmente utilizados:

`@GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping`

Estas anotaciones son creadas a partir de `@RequestMapping`, por ejemplo,
`@GetMapping` esta declarada así:

```
@RequestMapping(method=GET)
public @interface GetMapping
```

## Variables de ruta

```java
@GetMapping("ruta1/{name1}/{name2}")
public String getData(
  @PathVariable("name2") int age,
  @PathVariable("name1") String name
) {
  String formattedText = "La edad de %s es %d";
  return String.format(formattedText, name, age);
}
```

## Formato de solicitud HTTP

Una solicitud http tiene la siguiente estructura:

```
POST /api/v1/books HTTP/1.1
Content-Type: application/json
Content-Length: 72
Accept: application/json

{
  "title": "some title",
  "author": "some author",
  "price": 450.0
}
```

Comienza con una linea en donde se especifica el método HTTP, separado
con un espacio de la ruta a la que se hace la petición y de la versión del
protocolo HTTP que se esta usando.

Las siguientes lineas representan los `headers`, los headers se terminan
cuando hay una linea en blanco, despues de esa linea en blanco se encuentra
el cuerpo de la solicitud.

Para poder acceder al cuerpo de una solicitud en Spring se usa la anotación
`@RequestBody`, en base a los campos del json llena los campos del objeto que
coinciden en el nombre:

```java
@PostMapping("/books")
public Book addBook(@RequestBody Book book) {
  return addNewBook(book);
}
```

Usando la anotación @RequestBody solo se accede al cuerpo de la peticion, pero
para poder acceder a los headers y todos los datos de la peticion, se agrega
un parametro de tipo `HttpEntity<T>` donde T podemos especificar un tipo de dato
que esperamos leer del cuerpo de la petición, la siguiente petición es
equivalente a la aneterior, solo que ya se puede acceder a todos los datos de la
petición:

```java
@PostMapping("/books")
public Book addBook(HttpEntity<Book> entity) {
  entity.getHeaders();
  entity.hasBody();
  return addNewBook(entity.getBody(););
}
```

## Respuestas HTTP

Hay dos formas de crear una respuesta HTTP, una es devolviendo un tipo de dato
en el method handler que se convierte a un formato que pueda ser escrito en el
body, los headers y linea de inicio son generados automaticamente:

```java
@GetMapping("/books")
public Book getBooks() {
  return new Book();
}
```

La otra forma es devolver un tipo `ResponseEntity<T>` donde T es el tipo de
dato que sera escrito en el body de la respuesta HTTP. Con un objeto de tipo
ResponseEntity se puede controlar el codigo de estado de respuesta y los
headers:

```java
@RequestMapping("/handle")
public ResponseEntity<String> handle() {
  URI location = new URI();
  HttpHeaders responseHeaders = new HttpHeaders();
  responseHeaders.setLocation(location);
  responseHeaders.set("MyResponseHeader", "MyValue");
  return new ResponseEntity<String>("Hello World", responseHeaders, HttpStatus.CREATED);
}
```

Resultado:

```Response
HTTP/1.1 201 Created
MyResponseHeader: MyValue
Content-Type: text/plain
Content-Length: 11

Hello World
```

## Conversiones de tipos complejos

Al decir tipo complejo es hablar de tipos de datos que no son simples como
Integer, String, Boolean, Double; o estructuras de datos comunes como
List<T> o Map<K,V>, se refiere a otro tipo de datos o clases que podemos
generar mientras programamos.

Por ejemplo, podemos trabajar con objetos que representan la informacion de los usuarios
de nuestro backend y posiblemente tendremos las siguientes clases:

<!-- tabs:start -->

#### **Api.java**

src/main/java/package

```java
@RestController
@RequestMapping("/api/v1")
public class Api {

  @PostMapping("/users")
  public String addUser(@RequestBody User user) {
    return "Se agrego el usuario " + user.getFirstName();
  }

  // forma alternativa de recibir los datos
  @PostMapping("/users")
  public String addUser(@RequestBody Map<String, Object> map) {
    return "Se agrego el usuario " + map.get("firstName"));
  }
}
```

#### **User.java**

src/main/java/package

```java
public class User {
  private String firstName;
  private String lastName;
  private Integer age;
  private String email;
  
  public void setFirstName(String firstName) {
    this.firstName = firstName;
  }
  
  public String getFirstName() {
    return this.firstName;
  }
  
  public void setLastName(String lastName) {
    this.lastName = lastName;
  }
  
  public String getLastName() {
    return this.lastName;
  }
  
  public void setAge(Integer age) {
    this.age = age;
  }
  
  public Integer getAge() {
    return this.age;
  }
  
  public void setEmail(String email) {
    this.lastName = lastName;
  }
  
  public String getEmail() {
    return this.email;
  }
}
```

#### **Request/Response**

```
POST /api/v1/users HTTP/1.1
Content-Type: application/json
Content-Length: 88

{
  "firstName": "Juan",
  "lastName": "Diaz",
  "age": 18
  "email": "juan@gmail.com"
}
```

```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 25

Se agrego el usuario Juan
```

<!-- tabs:end -->

Spring usa la reflexion para generar la estructura de un json/xml para todos
los campos que tengan generado apropiadamente sus getters/setters haciendo una
conversion automatica hacia o desde json/xml cuando lee datos del body de una
peticion HTTP `@RequestBody` o cuando genera el body una respuesta HTTP
`@ResponseBody`.

El body se puede interpretar como un clave valor Map<String, Object>.
Se usa como clave String porque en JSON siempre en la clave se usa un dato
String mientras que para el valor se usa el tipo Object para indicar que
puede ser de cualquier tipo: Integer, String, Boolean, Double, List, Map, etc.

En el ejemplo anterior, con la anotacion @RequestBody se pude **parsear** el
**body** de un **request**. Pero igualmente al retornar cualquier tipo desde
un method handler, el objeto se codifica en su representacion equivalente a JSON
como en el siguiente ejemplo:

<!-- tabs:start -->

#### **Api.java**

src/main/java/package

```java
@RestController
@RequestMapping("/api/v1")
public class Api {

  @GetMapping("/company")
  public Company getCompanyInfo() {
    return new Company(
            "Google",
            new StreetAddress("Centro", 12, "12345"),
            List.of(
                    new Employee(1, "Juan", 49000.99),
                    new Employee(2, "Maria", 49000.99),
                    new Employee(3, "Pablo", 49000.99)
            )
    );
  }
}
```

#### **Company.java**

src/main/java/package

```java
public class Company {
  private String companyName;
  private StreetAddress streetAddress;
  private List<Employee> employees;
  
  public void setCompanyName(String companyName) {
    this.companyName = companyName;
  }
  
  public String getCompanyName() {
    return this.companyName;
  }
  
  public void setStreetAddress(StreetAddress streetAddress) {
    this.streetAddress = streetAddress;
  }
  
  public StreetAddress getStreetAddress() {
    return this.streetAddress;
  }
  
  public void setEmployees(List<Employee> employees) {
    this.employees = employees;
  }
  
  public List<Employee> getEmployees() {
    return this.employees;
  }
}
```

#### **StreetAddress.java**

src/main/java/package

```java
public class StreetAddress {
  private String streetName;
  private Integer streetNumber;
  private String zip;
  
  public void setStreetName(String streetName) {
    this.streetName = streetName;
  }
  
  public String getStreetName() {
    return this.streetName;
  }
  
  public void setStreetNumber(Integer streetNumber) {
    this.streetNumber = streetNumber;
  }
  
  public Integer getStreetNumber() {
    return this.streetNumber;
  }
  
  public void setZip(String zip) {
    this.zip = zip;
  }
  
  public String getZip() {
    return this.zip;
  }
}
```

#### **Employee.java**

src/main/java/package

```java
public class Employee {
  private Integer employeeId;
  private String name;
  private Double salary;
  
  public void setEmployeeId(Integer employeeId) {
    this.employeeId = employeeId;
  }
  
  public Integer getEmployeeId() {
    return this.employeeId;
  }
  
  public void setName(String name) {
    this.name = name;
  }
  
  public String getName() {
    return this.name;
  }
  
  public void setSalary(Double salary) {
    this.salary = salary;
  }
  
  public Double getSalary() {
    return this.salary;
  }
}
```

#### **Request/Response**

```
GET /api/v1/company HTTP/1.1

```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 396

{
  "companyName": "Google",
  "streetAddress": {
    "streetName": "Centro",
    "streetNumber": 12,
    "zip": "12345"
  },
  "employees": [
    {
      "employeeId": 1,
      "name": "Juan",
      "salary": 49000.99
    },
    {
      "employeeId": 2,
      "name": "Maria",
      "salary": 49000.99
    },
    {
      "employeeId": 3,
      "name": "Pablo",
      "salary": 49000.99
    }
  ]
}
```

<!-- tabs:end -->

## Consumiendo la API

Usando un cliente http como Postman o insomnia.

Envio de datos desde un formulario:

```html
<!--
<form
    action="https://www.google.com"
    method="POST" // por defecto GET
    enctype="application/x-www-form-urlencoded" // valor por defecto
    enctype="text/plain"
    enctype="multipart/form-data">
-->
<form action="/books" method="post">
  <label for="title">Title:</label><br>
  <input type="text" id="title" name="title"><br>
  <label for="author">Author:</label><br>
  <input type="text" id="author" name="author"><br><br>
  <label for="price">Price:</label><br>
  <input type="text" id="price" name="price"><br><br>
  <input type="submit" value="Enviar">
</form>
```

Peticion HTTP desde javascript:

```javascript
function getBooks() {
  fetch("/books")
    .then(response => response.json())
    .then(books => console.log(books));
}
```

