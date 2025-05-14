# Snippets de Animaciones

```dart
AnimatedSwitcher(
  duration: const Duration(milliseconds: 300),
  transitionBuilder: Widget Function(Widget, Animation<double>),
  child: Widget,
)
```

## Transformar Animation con Tween

```dart
Animation<double> animation = ...;
Animation<Offset> position = animation.drive(Tween<Offset>(
  begin: Offset(-1.0, 0.0),
  end: const Offset(0.0, 0.0),
));
```

## Transition

En el widget `SlideTransition` los valores 0 en offset significan que no hay
cambio en la posicion del widget, los valores son proporcionales al tamaño del
widget, 1 es el desplazamiento igual al tamaño del widget dx para el ancho y
dy para el alto; dx 1 positivo es movimiento a la derecha, negativo a la
izquierda:

```dart
SlideTransition(
  position: Animation<Offset>,
  child: Widget,
)
```

<p align="center">
  <img src="/images/flutter_offset.svg" alt="SlideTransition Diagram" />
</p>
