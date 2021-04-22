# Notificaciones Push en Flutter

## Integración en Flutter 1.12 con v2 Android embedding

Acceder a Firebase y registrar el paquete de la aplicación. Agregar en la ruta
`android/app/google-services.json` la configuracion de firebase.

Despues en el archivo `android/build.gradle` la siguientes lineas:

```gradle
buildscript {
    // resto de lineas

    dependencies {
        // resto de lineas
        classpath 'com.google.gms:google-services:4.3.5' // Agregar esta linea
    }
}

allprojects {
    repositories {
        google() // verificar la existencia de esta linea
        jcenter()
    }
}
// resto del codigo
```

Despues en el archivo `android/app/build.gradle` la siguientes lineas:

```gradle
// resto de lineas
apply plugin: 'com.android.application'
apply plugin: 'com.google.gms.google-services' // Agregar esta linea

// resto del codigo

dependencies {
    // resto de lineas
    implementation platform('com.google.firebase:firebase-bom:27.1.0') // Agregar esta linea
    implementation 'com.google.firebase:firebase-analytics-ktx' // Agregar en caso de haber agregado analytics en el proyecto firebase
}
```

En el `AndroidManifest` agregar el siguiente intent filter dentro de la etiqueta
`activity`, especificamente dentro del activity no dentro de la etiqueta
`application`:

```xml
<intent-filter>
    <action android:name="FLUTTER_NOTIFICATION_CLICK"/>
    <category android:name="android.intent.category.DEFAULT"/>
</intent-filter>
```

En el archivo `pubspec.yml` agregar las siguientes dependencias:

```
firebase_messaging: ^7.0.3
firebase_core: ^0.5.0+1
```

Una vez eso hecho se puede remplazar el codigo dentro de archivo `main.dart` por
el siguiente fragmento codigo para imprimir un mensaje cuando llegue una
notificacion solo si la aplicacion se encuentra en primer plano, en segundo
plano solo se recibe una notificacion en la barra de notificaciones pero no es
posible ejecutar codigo dart aun:

```dart
import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter/cupertino.dart';

void main() async {
  runApp(Aplicacion());
}

class Aplicacion extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    FirebaseMessaging messaging = FirebaseMessaging();
    messaging.getToken().then(print);
    messaging.configure(
      onMessage: (Map<String, dynamic> message) async {
        print('Acaba de llegarme una notificacion con los siguientes datos $message');
        return null;
      }
    );
    return CupertinoApp(
      title: 'FCM Demo',
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  HomeScreen({Key key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text('FCM Demo'),
      ),
      child: Center(
        child: Icon(CupertinoIcons.airplane),
      ),
    );
  }
}

```

Ultima actualizacion: 22 de abril

