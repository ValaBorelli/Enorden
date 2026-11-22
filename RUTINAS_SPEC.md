# EnOrden — especificación funcional de Rutinas

Estado: reglas funcionales acordadas para el módulo Rutinas. Los aspectos que requieren una definición adicional están identificados en la sección de pendientes.
Rama de desarrollo: EnOrden-Desarrollo.
Este documento reúne las reglas específicas aprobadas para el módulo Rutinas. No debe interpretarse como autorización para publicar en producción.
Las reglas de este documento describen el comportamiento esperado; no implican que las funcionalidades ya estén implementadas.

## 1. Estructura principal del módulo

- La página principal de Rutinas muestra las rutinas creadas por el usuario.
- Cada rutina funciona visualmente como una carpeta que contiene sus propias tareas.
- Las tareas de Rutinas son independientes de las tareas del módulo Tareas. No se mezclan ni aparecen automáticamente en ese otro módulo.
- Al tocar una rutina, se despliega su contenido y se muestran las tareas que contiene.
- Al mantener presionada una rutina, se abre su ventana de configuración.
- Cada rutina tiene su propia configuración, programación, estado de activación y secuencia de tareas.
- Modificar una rutina no debe alterar las demás.

## 2. Creación y configuración de una rutina

- Al crear una rutina, el usuario puede configurar su nombre.
- El usuario puede establecer cuándo debe comenzar a funcionar la rutina.
- La configuración permite establecer fechas específicas, períodos de funcionamiento o una programación repetitiva.
- El usuario puede establecer la hora de activación automática de la rutina.
- También puede dejar una rutina sin fechas ni horarios programados, para encenderla y apagarla manualmente.
- Cada rutina puede activarse o desactivarse manualmente.
- Las rutinas pueden configurarse y programarse independientemente, incluso cuando existan varias rutinas creadas para distintos horarios o turnos.

## 3. Creación y configuración de tareas dentro de una rutina

- Cada rutina puede contener una o varias tareas.
- Al crear una tarea, el usuario puede configurar:
  - Nombre de la tarea.
  - Duración de la tarea.
- Las tareas tienen un orden definido dentro de su rutina.
- Al tocar una tarea, se abre su ventana de edición, equivalente a la ventana de creación.
- Desde esa ventana se puede modificar el nombre y la duración de la tarea.
- Al mantener presionada una tarea, el usuario puede cambiar su posición dentro de la rutina.
- El cambio de orden permite reorganizar la secuencia en la que se ejecutarán las tareas.

## 4. Funcionamiento y secuencia de las tareas

- Cuando una rutina se activa, comienza automáticamente desde su primera tarea.
- Al comenzar la rutina, suena una alarma o notificación que indica al usuario que debe iniciar la primera tarea.
- Cada tarea tiene una duración previamente establecida.
- Cuando se cumple la duración de una tarea, comienza inmediatamente la siguiente y se emite la correspondiente alarma.
- El avance de las tareas es automático. No es necesario que el usuario marque una tarea como terminada para que comience la siguiente.
- La duración de cada tarea determina cuándo debe comenzar la siguiente, respetando las modificaciones de tiempo que el usuario haya realizado mediante la opción de posponer.
- La secuencia debe respetar las reglas de posposición del apartado 5: posponer una tarea no desplaza automáticamente los horarios originales de las demás tareas.

## 5. Función de posponer alarmas

- Las alarmas que anuncian el comienzo de una tarea tienen una opción para posponer.
- El usuario puede posponer una alarma por intervalos de 5 o 10 minutos.
- Cada alarma tiene un máximo de tres posposiciones.
- El límite de tres posposiciones se aplica individualmente a cada tarea, independientemente de la rutina a la que pertenezca.
- Cuando el usuario pospone una alarma, solamente se retrasa el comienzo de la tarea correspondiente.
- Las demás tareas de la rutina conservan sus horarios originales y no se retrasan automáticamente por la posposición de otra tarea.
- Si el usuario pospone varias tareas de una misma rutina, cada una tiene su propio retraso independiente.
- Si el usuario pospone todas las tareas de una rutina, todas se retrasan individualmente según los minutos de posposición que haya seleccionado para cada una.
- Posponer una tarea no modifica la programación original de las demás tareas ni la hora de activación de otras rutinas.

## 6. Apagado y reactivación manual de una rutina

- Si el usuario apaga manualmente una rutina mientras está funcionando, se detiene su ejecución.
- Al apagarla, dejan de generarse las alarmas correspondientes a las tareas pendientes de esa ejecución.
- La rutina permanece apagada hasta que el usuario vuelva a activarla o la configure para una nueva fecha.
- Si el usuario apaga manualmente una rutina y posteriormente vuelve a encenderla, esta comienza desde su primera tarea.
- No se retoma la tarea en la que había quedado antes de apagarse.
- Cuando una rutina se activa mediante una programación automática, también comienza siempre desde la primera tarea.
- Si el usuario apaga manualmente una rutina que tiene períodos futuros programados, el sistema debe preguntarle si desea:
  - Apagar únicamente la ejecución del período actual.
  - Apagar la rutina para todos los períodos siguientes.
- Si el usuario selecciona apagar solamente el período actual, se detiene la ejecución vigente, pero se conservan las programaciones y eventos de los períodos futuros.
- Si el usuario selecciona apagar la rutina para todos los períodos siguientes, se cancelan las próximas activaciones automáticas de esa rutina.
- Al cancelar todos los períodos siguientes, los eventos correspondientes a esos períodos se eliminan automáticamente del calendario.
- La eliminación de esos eventos no debe afectar los eventos de otras rutinas.

## 7. Programación por fechas y períodos

- El usuario puede establecer que una rutina funcione durante un período determinado.
- Por ejemplo, puede configurarla para funcionar desde el lunes hasta el viernes.
- También puede establecer fechas específicas de inicio y término.
- Una rutina puede tener períodos de funcionamiento diferentes de los de otras rutinas.
- La programación determina automáticamente cuándo debe activarse la rutina, sin que el usuario tenga que encenderla cada día.
- Los períodos programados deben respetar la hora de activación establecida para cada rutina.

## 8. Programación repetitiva y turnos rotativos

- El usuario puede configurar una rutina para que se active automáticamente con una frecuencia determinada.
- La frecuencia podrá establecerse, como mínimo, en:
  - Cada semana.
  - Cada dos semanas.
  - Cada tres semanas.
- La programación repetitiva permite establecer cuándo comienza el ciclo y durante cuánto tiempo debe mantenerse activa la rutina en cada repetición.
- Esto permitirá configurar rutinas para turnos laborales rotativos.
- Ejemplo de programación de turnos:

  | Semana | Rutina | Hora de activación |
  | --- | --- | --- |
  | Semana 1 | Turno 7:00 AM | 05:00 AM |
  | Semana 2 | Turno 10:00 AM | 08:00 AM |
  | Semana 3 | Turno 4:00 PM | 02:00 PM |
  | Semana 4 | Turno 7:00 AM | 05:00 AM |

- Cada rutina puede tener su propia frecuencia y período de activación.
- El sistema debe permitir que las programaciones repetitivas continúen funcionando según el ciclo establecido, sin que el usuario tenga que volver a configurarlas cada semana.
- La programación de turnos rotativos mediante ciclos de distintas rutinas todavía requiere una definición específica. Consultar la sección de pendientes antes de implementar esa modalidad.

## 9. Integración automática con el calendario

- Cuando una rutina tiene un período de funcionamiento programado, el sistema crea automáticamente sus eventos en el calendario.
- La creación de estos eventos es obligatoria y automática. No se le pregunta al usuario si desea agregarlos.
- Se crea un evento por cada día en que la rutina esté programada para funcionar.
- Los eventos tienen la identificación «Evento Rutina».
- Ejemplo: si una rutina está programada de lunes a viernes, se crean cinco eventos, uno para cada día.
- Aunque existan varios eventos consecutivos, el calendario envía solamente una notificación previa al comienzo del período.
- La notificación se envía el día anterior al inicio del período programado.
- La notificación indica:
  - Que la rutina comenzará al día siguiente.
  - El nombre de la rutina.
  - El período durante el cual estará activa, indicando desde qué día hasta qué día.
- Ejemplo: si la rutina comienza el lunes y termina el viernes, el domingo el usuario recibe una sola notificación indicando que el lunes comienza esa rutina y que estará activa de lunes a viernes.
- Si la rutina tiene una programación repetitiva, cada nuevo período de activación tiene una sola notificación previa, enviada el día anterior a su comienzo.
- Los eventos del calendario no generan alarmas para las tareas.
- Las alarmas de inicio y transición entre tareas son responsabilidad exclusiva del módulo Rutinas.
- Si una rutina se activa y se apaga exclusivamente de forma manual, no se crean eventos en el calendario.
- En consecuencia, las rutinas de activación exclusivamente manual no generan notificaciones previas del calendario.
- Si el usuario cancela todos los períodos futuros de una rutina, los eventos correspondientes a esos períodos se eliminan automáticamente del calendario.

## 10. Detección de rutinas simultáneas

- El sistema debe comprobar si existen dos o más rutinas programadas para emitir alarmas a la misma hora.
- Si detecta que dos o más rutinas tienen tareas cuyas alarmas coincidirán exactamente, debe identificar el conflicto.
- El usuario recibe una notificación indicando que existen rutinas con tareas programadas para sonar simultáneamente.
- La notificación identifica todas las rutinas involucradas en el conflicto.
- El sistema solicita al usuario que seleccione solamente una rutina que desea mantener activa.
- Todas las demás rutinas involucradas en ese conflicto se apagan automáticamente.
- El usuario debe ser informado de cuáles rutinas permanecerán activas y cuáles serán apagadas.
- La notificación recomienda modificar los horarios o las programaciones desde la configuración de las rutinas para evitar futuros conflictos.
- El sistema debe resolver el conflicto antes de permitir que las rutinas involucradas continúen activas con alarmas simultáneas.

## 11. Alarmas y funcionamiento con la aplicación cerrada

- Las alarmas deben poder funcionar aunque EnOrden no esté abierta en primer plano.
- También deben poder funcionar cuando el teléfono esté bloqueado.
- No es suficiente utilizar temporizadores que dependan de que la página del módulo permanezca abierta.
- Se debe implementar un sistema de notificaciones y programación de alarmas compatible con el dispositivo.
- La implementación debe contemplar:
  - Permisos de notificaciones.
  - Permisos de alarmas, cuando corresponda.
  - Restricciones de ejecución en segundo plano.
  - Limitaciones del sistema operativo.
  - Persistencia de las alarmas programadas.
  - Cancelación o reprogramación de alarmas cuando una rutina se apague, cambie o se reinicie.
  - Actualización de las alarmas cuando una tarea sea pospuesta.
- El comportamiento debe ser confiable tanto en dispositivos Android como en otros dispositivos compatibles que se incorporen en el futuro.

## 12. Plantillas de rutinas

- El módulo Rutinas tendrá una sección propia de plantillas.
- Las plantillas permitirán guardar configuraciones de rutinas para volver a utilizarlas.
- Las plantillas de Rutinas serán independientes de las plantillas del módulo Tareas.
- La plantilla deberá conservar, como mínimo, la estructura de la rutina y sus tareas, incluyendo:
  - Nombre de las tareas.
  - Duración de las tareas.
  - Orden de las tareas.

## 13. Pendientes de definición

Los siguientes puntos todavía requieren una decisión o definición adicional. No deben implementarse mediante suposiciones.

### 13.1. Posposición de alarmas

**Resuelto.**
- Cada tarea tiene su propia alarma.
- Cada alarma puede posponerse un máximo de tres veces.
- Las opciones de posposición son 5 o 10 minutos.
- El retraso afecta exclusivamente a la tarea cuya alarma se pospuso.
- Las demás tareas mantienen sus horarios originales.

### 13.2. Programación de turnos rotativos

**Pendiente de definición.**
- Se debe revisar cómo configurar una secuencia de distintas rutinas que se alternan semanalmente.
- Ejemplo: semana 1, rutina A; semana 2, rutina B; semana 3, rutina C; semana 4, rutina A nuevamente.
- No implementar todavía una secuencia/ciclo de distintas rutinas sin definir previamente su configuración y comportamiento.

### 13.3. Eventos y notificaciones del calendario

**Resuelto.**
- Los períodos programados crean eventos automáticamente.
- Se crea un evento por cada día del período.
- Se envía una sola notificación previa por período.
- La notificación se envía el día anterior al comienzo.
- La notificación indica el nombre de la rutina y el período completo.
- Las rutinas exclusivamente manuales no crean eventos ni notificaciones del calendario.

### 13.4. Conflictos entre rutinas

**Resuelto.**
- Si existen dos o más rutinas con alarmas simultáneas, el usuario debe seleccionar solamente una para mantenerla activa.
- Las demás rutinas involucradas se apagan.
- Se informa al usuario de lo ocurrido y cuáles rutinas se mantienen activas o se apagan.
- Se recomienda modificar los horarios o las programaciones para evitar nuevos conflictos.

### 13.5. Apagado manual y programación automática

**Resuelto.**
- Cuando se apaga manualmente una rutina con períodos futuros programados, se pregunta al usuario si desea apagar solamente el período actual o también todos los períodos siguientes.
- Si elige apagar todos los períodos siguientes, se cancelan sus futuras activaciones y se eliminan automáticamente sus eventos del calendario.
- Si elige apagar solamente el período actual, se conserva la programación y los eventos de los períodos futuros.
