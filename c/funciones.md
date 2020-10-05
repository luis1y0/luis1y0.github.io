# Funciones

En lenguaje C las funciones se declaran de la siguiente forma:

```c
#include <stdio.h>

int power(int base, int potencia) {
  int b = 1;
  for (;potencia > 0; potencia --) b *= base;
  return b;
}

int main() {
  int base = 2;
  int p = 5; 
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  p += 3;
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  return 0;
}
```

En el codigo anterior se define una funcion con nombre `power` que retorna un tipo int y tiene 2 parametros, tales
parametros son una copia de los argumentos pasados en la funcion main, es por ello que la variable `potencia` aun
siendo decrementada en el ciclo for no hace efecto en la variable `p`, ese comportamiento es de las variables primitivas
del lenguaje; una vez ejecuta la funcion, su evaluacion resulta en un numero entero.

Ahora si se escribieran las mismas funciones pero en orden diferente como el se ve a continuacion no se podria compilar:

```c
#include <stdio.h>

int main() {
  int base = 2;
  int p = 5; 
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  p += 3;
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  return 0;
}

int power(int base, int potencia) {
  int b = 1;
  for (;potencia > 0; potencia --) b *= base;
  return b;
}
```

Al intentar compilar se obtiene la siguiente salida:

```
funciones.c: In function ‘main’:
funciones.c:6:44: warning: implicit declaration of function ‘power’ [-Wimplicit-function-declaration]
    6 |   printf("%d^%d es igual a %d\n", base, p, power(base, p));
      |
```

Eso se debe a que en lenguaje C todas las funciones al momento de llamarlas como es el caso de llamar a `power(base, p)`
dentro de la función main, esa funcion debio ser declarada previamente de lo contrario no podra compilarse.

La solucion usar una `función prototipo`, que simplemente es escribir la *firma* de la funcion al principio del archivo
para que el compilador la reconozca como una funcion valida dentro del archivo y pueda ser llamada:

```c
#include <stdio.h>

int power(int, int);

int main() {
  int base = 2;
  int p = 5; 
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  p += 3;
  printf("%d^%d es igual a %d\n", base, p, power(base, p));
  return 0;
}

int power(int base, int potencia) {
  int b = 1;
  for (;potencia > 0; potencia --) b *= base;
  return b;
}
```

La partes necesarias de una `función prototipo` o `firma` son el tipo de dato de retorno, el nombre de la funcion y el
tipo de datos que tiene como parametros(el nombre de los parametros es opcional y no es necesario que sean iguales la
declaracion de la funcion).
