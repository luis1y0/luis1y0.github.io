# Quickstart

## Comandos principales y comenzar proyecto

Para generar un nuevo componente:

```bash
ng generate component xyz # forma normal
ng g c xyz # forma abreviada
```

Para generar un nuevo modulo:

```bash
ng generate module xyz
```

Tambien se pueden generar:

```
app-shell
application
class
component
directive
enum
guard
interceptor
interface
library
module
pipe
resolver
service
service-worker
web-worker
```

Agregar Angular Material y soporte para PWA, son dependencias, otras
dependencias se agregan usando `ng add _____  `:

```bash
ng add @angular/material
ng add @angular/pwa
```

Generar el producto para desplegar a producción:

```bash
ng build
```

Como aplicación de ejemplo se tiene lo siguiente en el archivo
`src/app/app.component.ts`:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

En el archivo `src/app/app.component.html`:

```html
<h1>{{title}}</h1>
```

## Crear componente Hero Editor

Ejecutar comando:

```bash
ng generate component heroes
```

Se genera una carpeta en `src/app/heroes` con los 3 archivos necesarios
`heroes.component.ts`, `heroes.component.html`, `heroes.component.css`.

Los archivos `<name>.component.ts` es donde inicia la ejecución del componente,
de ahi se generan las variables y comportamiento del componente y con la
anotación @Component automaticamente se renderiza el template en el archivo
declarado en la propiedad `templateUrl` y sus estilos en `styleUrls`:

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  hero = 'Windstorm';

  constructor() { }

  ngOnInit(): void {
  }

}
```

En el archivo `src/app/heroes/heroes.component.html` editar el contenido:

```
<h2>{{hero}}</h2>
```

Como en la propiedad `selector` del archivo `src/app/heroes/heroes.component.ts`
se coloco 'app-heroes', se puede renderizar desde otro template con la etiqueta
`<app-heroes></app-heroes>`, pero antes de poder utilizar la etiqueta, el
componente debe ser declarado en el modulo para poder llamarlo desde otros
modulos, antes se hace la declaración en el archvo `src/app/app.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';

@NgModule({
  declarations: [
    AppComponent,
    // declaracion del componente para ser usado desde otros componentes
    HeroesComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Con el selector `app-heroes` se renderiza en `src/app/app.component.html`:

```html
<h1>{{title}}</h1>
<app-heroes></app-heroes>
```

## Resumen

Cada componente se compone de 3 archivos, un componente en typescript,
desde el que se renderiza un template html y al que se enlazan estilos
css o de otro tipo.

Cada que se genera un nuevo componente, se debe declarar en el modulo al
que pertenece para poder ser llamado desde otro componente o renderizado desde
el template de otro componente.

