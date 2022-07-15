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
  @Autowired
  UserService userService;
  @GetMapping("/users/length")
  public int getNumberOfUsers() {
    // si se devuelve un primitivo, el valor en texto sera devuelto directamente
    // en el cuerpo de la respuesta HTTP
    return this.userService.getUsers().length;
  }
  @GetMapping("/users/{id}")
  public User getUserById(@PathVariable int id) {
    // Cuando es un objeto no primitivo, se convierte automaticamente en una
    // estructura clave valor en base a los campos de clase: {"age": 25}
    return this.userService.getUserById(id);
  }
  @GetMapping("/users")
  public ArrayList<User> getBook(@PathVariable int id) {
    // Si es una coleccion de clases, automaticamente se genera un arreglo
    // con los elementos representados como clave valor: [{"age": 18}, {"age": 25}]
    return this.userService.getUsers();
  }
}
```

@RestController fue introducido en Spring 4.0 para 
simplificar la creacion de servicios web RESTful (es un anotacion
conveniente que combina @Controller y @ResponseBody, que elimina la necesidad
de anotar cada metodo de manejo de request con @ResponseBody).

Despues de clasificar a una clase con @RestController se debe indicar la ruta
base a partir de la cual todos los metodos de la clase usan, si no se
especifica, el valor es un string vacio que igual a la raiz de la ruta HTTP.

Este controlador usando @Controller:

```java
@Controller
@RequestMapping("books")
public class ClassName {
  @GetMapping("/{id}", produces = "application/json")
  public @ResponseBody Book getBook(@PathVariable int id) {
    return findBookById(id);
  }
}
```

Es equivalente a usar @RestController:

```java
@RestController
// @RequestMapping("ruta", produces = {MediaType.APPLICATION_JSON_VALUE})
@RequestMapping
public class ClassName {
  @GetMapping("/{id}", produces = "application/json")
  public Book getBook(@PathVariable int id) {
    return findBookById(id);
  }
}
```

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

## Parametros HTTP

Los parametros HTTP son los variables que se pueden incluir al final de
una URL despues del signo `?`, ejemplo:
`https://www.google.com/search?q=spring+framework&sourceid=chrome`. Las
q y sourceid en Spring Boot se extraen con la anotacion `@ParamRequest`:

```java
@GetMapping("search")
public String getData(
  @RequestParam("q", defaultValue="Hello") String query,
  @RequestParam("sourceid", defaultValue="World") String browserName
) {
  String formattedText = "Buscando %s desde %s";
  return String.format(formattedText, query, browserName);
}
```

## Cuerpo de solicitud HTTP

## Consumiendo la API

```html
<form>
</form>
```

```javascript
fetch("url").then(response => response.json());
```

