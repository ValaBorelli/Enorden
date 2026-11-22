# EnOrden — especificación funcional de Calendario

Estado: reglas funcionales acordadas para el módulo Calendario.
Rama de desarrollo: EnOrden-Desarrollo.
Este documento reúne las reglas específicas aprobadas para el módulo Calendario. No debe interpretarse como autorización para publicar en producción.

## 1. Apertura y posición inicial de las ventanas

- Cada nueva creación de evento debe abrirse desde el principio de la ventana, nunca desde la posición en la que quedó la creación anterior.

## 2. Confirmación de creación de eventos

- Cada vez que se cree y guarde correctamente un evento nuevo desde el módulo Calendario, debe mostrarse una ventana de confirmación.
- La ventana debe utilizar la estructura y el diseño base de la categoría `confirmation` del sistema general de ventanas de EnOrden.
- La confirmación debe aparecer únicamente después de que el evento se haya guardado correctamente.
- Esta regla aplica a la creación de eventos nuevos; editar un evento existente no debe mostrar el mensaje de creación.

## 3. Confirmación de edición de eventos

- Cada vez que se edite y guarde correctamente un evento existente desde el módulo Calendario, debe mostrarse una ventana de confirmación.
- La ventana debe utilizar la estructura y el diseño base de la categoría `confirmation` del sistema general de ventanas de EnOrden.
- La confirmación debe aparecer únicamente después de guardar correctamente los cambios.
- El mensaje debe indicar que el evento fue actualizado correctamente e incluir su nombre.
- Esta confirmación es independiente de la confirmación de creación y solo debe mostrarse al editar un evento existente.
