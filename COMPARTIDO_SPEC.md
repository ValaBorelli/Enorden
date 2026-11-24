# EnOrden — Especificación funcional de Compartido

Estado: definición acordada para la primera versión local.
Rama de desarrollo: EnOrden-Desarrollo.
Este documento es la referencia funcional para implementar el módulo Compartido. No debe interpretarse como autorización para publicar en producción.

## 1. Propósito y separación

Compartido es un sistema independiente de proyectos colaborativos. No es una categoría de las tareas normales ni una extensión de Personal o Familia.

- Cada usuario tiene una única página/espacio Compartido.
- En esa página ve un listado de proyectos que creó y proyectos de otras personas en los que participa y tiene acceso.
- Los proyectos no son tareas normales y no deben aparecer en Personal, Familia, Mi día, Todas, Asignadas, calendario ni plantillas de tareas normales.
- Desde Compartido no se pueden crear tareas normales.
- Las tareas y subtareas propias de un proyecto pertenecen al sistema del proyecto y no se mezclan con el módulo Tareas.
- La creación de proyectos se realiza exclusivamente desde la página principal Compartido.
- La categoría heredada «Tareas Compartido» del editor de tareas normales debe retirarse al integrar el nuevo sistema de proyectos. Los registros antiguos con scope=shared no deben convertirse silenciosamente en proyectos ni mezclarse con ellos; cualquier migración debe tratarse explícitamente.

## 2. Página principal Compartido

La página principal muestra únicamente un listado de proyectos; no muestra tareas ni subtareas individuales.

El listado incluye:
- Proyectos creados por el usuario.
- Proyectos creados por otras personas a los que el usuario tiene acceso.

Las secciones de la lista se titulan exactamente «Proyectos anclados» y «Otros proyectos».

Organización individual por usuario:
- Cada usuario puede ordenar manualmente sus proyectos.
- Los proyectos nuevos o recién incorporados aparecen al principio de la lista no anclada.
- El usuario puede anclar proyectos.
- Los proyectos anclados siempre aparecen antes que los no anclados, aunque lleguen proyectos nuevos.
- El orden y los anclajes son preferencias individuales y no alteran el orden de los demás participantes.

## 3. Creación y páginas de proyecto

- Los proyectos se crean únicamente desde la página principal Compartido.
- Cada proyecto tiene una página individual.
- Todas las páginas individuales se generan a partir de una plantilla común diseñada por el equipo de EnOrden.
- El usuario abre un proyecto presionando su nombre en el listado.
- La página principal Compartido continúa mostrando solamente proyectos, no el contenido interno de cada proyecto.
- El contenido y la organización interna de los proyectos se definirán e implementarán por etapas.
- La creación de proyectos es independiente de la creación/edición de tareas normales.

### 3.1. Ventana de creación del proyecto

- Al seleccionar «Crear proyecto», se abre una página provisional del proyecto con una ventana centrada titulada exactamente «Nuevo Proyecto».
- La ventana contiene el campo «Nombre del proyecto» y los botones «Guardar», «Cancelar» y «X».
- El nombre ingresado será el nombre definitivo del proyecto y el que se mostrará en la lista de Proyectos Compartidos.
- «Guardar» registra el proyecto y deja al usuario dentro de su página individual.
- «Cancelar» abre una confirmación preguntando si desea cancelar la creación, con las opciones «Sí, cancelar» y «Seguir creando».
- «Sí, cancelar» descarta la página provisional y vuelve a Proyectos Compartidos. «Seguir creando» cierra la confirmación y mantiene abierta la creación.
- La «X» cancela directamente la creación, descarta la página provisional y vuelve a Proyectos Compartidos.
- Pulsar fuera de la ventana no realiza ninguna acción ni la cierra.

### 3.2. Confirmación al salir de un proyecto

- Si el usuario intenta salir de una página individual de proyecto mediante cualquier botón de la navegación inferior, debe mostrarse una ventana con la pregunta exacta: «Quieres guardar antes de salir del proyecto?».
- La ventana ofrece «Guardar», «Salir sin guardar», «Cancelar» y una «X».
- «Guardar» conserva los cambios pendientes y permite salir del proyecto.
- «Salir sin guardar» descarta los cambios pendientes y permite salir.
- «Cancelar» y la «X» cierran únicamente la confirmación y mantienen al usuario dentro del proyecto.
- Si la salida se solicita desde una creación todavía no guardada, «Guardar» debe validar que exista un nombre antes de registrar el proyecto; «Salir sin guardar» descarta la creación provisional.
- Si la salida se solicita mientras se edita el nombre del proyecto, «Guardar» confirma el nuevo nombre y «Salir sin guardar» descarta ese cambio pendiente.

## 3.3. Estructura de la página individual del proyecto

La página individual tendrá tres espacios principales, independientes del resto de EnOrden:

1. **Carpetas y tareas:** organización del trabajo y responsabilidades.
2. **Pizarra del equipo:** espacio visual para recordatorios y anotaciones espontáneas.
3. **Chat del proyecto:** conversación grupal entre todos los participantes.

- Los tres espacios pertenecen exclusivamente al proyecto y no se conectan con las tareas normales, el calendario, las notas ni el chat general de EnOrden.
- La página se genera desde una plantilla común de EnOrden y debe conservar una presentación consistente en teléfonos, tabletas y computadores.
- Los controles visuales, botones, menús, ventanas, selectores y demás elementos de interfaz deben ser componentes propios de EnOrden siempre que la plataforma lo permita. No se deben utilizar controles nativos del navegador o del sistema operativo como sustitutos de componentes que puedan implementarse dentro de la aplicación.
- Las limitaciones inevitables del sistema operativo (por ejemplo, el teclado virtual o físico utilizado para introducir texto) no deben confundirse con la interfaz de EnOrden; la aplicación debe controlar su editor, presentación, validaciones y acciones.

## 3.4. Reglas generales de participación

- Los permisos de lectura y escritura permiten consultar y modificar la organización de tareas del proyecto.
- Los participantes con permiso de solo lectura pueden consultar carpetas y tareas, pero no crear, editar, mover ni eliminar tareas o carpetas.
- Todos los participantes con acceso al proyecto, incluidos los de solo lectura, pueden leer y enviar mensajes en el chat del proyecto.
- Todos los participantes con acceso al proyecto, incluidos los de solo lectura, pueden leer y publicar anotaciones en la pizarra compartida.
- La participación en el chat o la pizarra no concede permisos para modificar tareas, carpetas, participantes, invitaciones ni administración del proyecto.

## 4. Duración y vencimiento

Reglas iniciales:
- Todo proyecto tiene una duración máxima de 7 días.
- Los 7 días se cuentan desde la fecha y hora de creación del proyecto.
- Un día antes del vencimiento se debe notificar a todos los usuarios que tengan acceso al proyecto, avisando que será eliminado al cumplirse el plazo.
- Al cumplirse los 7 días, el proyecto vence y debe eliminarse automáticamente para todos los participantes.
- La eliminación automática no debe depender de que el creador o los participantes abran la aplicación.
- En una futura versión, el creador podrá elegir una duración mayor y modificarla mientras el proyecto siga activo y no haya sido eliminado manualmente ni por vencimiento. La duración configurable no forma parte de la primera versión.

## 5. Eliminación manual

- Solo el creador del proyecto puede eliminarlo.
- La eliminación manual solo está disponible desde dentro de la página individual del proyecto.
- La acción se realiza mediante un botón o control específico.
- Antes de eliminar, se muestra una ventana de confirmación para evitar accidentes.
- Si se confirma, el proyecto y su contenido asociado dejan de estar disponibles para todos los participantes y desaparecen de sus listados Compartido.
- Los participantes que no son creadores no pueden eliminar el proyecto.

## 6. Enlaces y solicitudes de acceso

- Cada proyecto tiene un único enlace de invitación, generado desde la página individual una vez creado el proyecto.
- El mismo enlace se utiliza para todas las personas invitadas a ese proyecto; no se genera un enlace diferente por persona.
- Al abrirlo, la persona debe iniciar sesión o registrarse en EnOrden.
- Abrir el enlace no concede acceso automáticamente.
- La persona queda en espera hasta que el creador/administrador autorice o rechace su solicitud.
- Las solicitudes son individuales: el administrador decide por separado para cada usuario.
- Cada usuario puede realizar como máximo 3 solicitudes para ese proyecto.
- Después de enviar una solicitud, debe esperar la respuesta del administrador antes de poder enviar otra solicitud.
- Si la solicitud es rechazada, puede volver a solicitar acceso si aún no ha alcanzado el máximo de 3 solicitudes. Si es aceptada, obtiene acceso y ya no necesita solicitarlo.
- Una persona no puede ser asignada a una tarea o subtarea del proyecto antes de tener acceso al proyecto.

## 7. Participantes, asignaciones y permisos

Los participantes pueden ser:
- El creador.
- Integrantes de la familia del creador.
- Personas externas a la familia.

La pertenencia a una familia no determina por sí sola el acceso al proyecto.

Distinguir siempre:
- Acceso: autorización para ver el proyecto.
- Asignación: responsabilidad sobre una tarea o subtarea.
- Permisos: acciones que un participante puede realizar dentro del proyecto.

El creador controla los permisos individuales de los demás participantes, siguiendo una lógica de permisos por usuario inspirada en Google Drive. Los niveles y acciones exactos se especificarán antes de implementar la matriz de permisos.

- Los participantes solo pueden ser asignados cuando ya tienen acceso al proyecto.
- Los usuarios con permiso de asignación pueden asignar a otros participantes a tareas y subtareas.
- Tener permiso para asignar no concede permiso para invitar, aprobar solicitudes ni modificar los permisos de acceso.
- Solo el creador/administrador autorizado puede conceder o cambiar permisos.
- El creador puede asignar a otro participante actual el permiso de administrador.
- El creador o administrador que quiera salir sin eliminar el proyecto debe transferir primero la administración a otro participante que ya pertenezca al proyecto.
- Dentro de la página del proyecto existirá un botón «Abandonar proyecto».
- La salida del creador no debe eliminar el proyecto si la administración fue transferida correctamente.
- La lógica de administración debe ser coherente con el modelo general de roles de EnOrden.

### 7.1. Carpetas y tareas internas del proyecto

- Cada proyecto tendrá carpetas personalizables creadas por sus participantes con permiso de lectura y escritura.
- En la vista principal del proyecto, las carpetas se mostrarán cerradas en una cuadrícula de tres columnas; las siguientes filas continuarán de izquierda a derecha.
- Al seleccionar una carpeta, se abrirá una vista interior dedicada a esa carpeta. No se expandirá la carpeta dentro de la lista principal.
- La vista interior de una carpeta tendrá un control de regreso con flecha en la zona superior izquierda para volver a la vista principal del proyecto.
- En la vista principal, debajo de todas las carpetas se mostrarán las tareas sueltas que no pertenezcan a ninguna carpeta.
- Una tarea creada desde la vista principal del proyecto quedará inicialmente sin carpeta. Una tarea creada desde el interior de una carpeta pertenecerá a esa carpeta.
- Las carpetas serán de un solo nivel: no podrán contener otras carpetas.
- Las carpetas podrán renombrarse, reordenarse y eliminarse según los permisos correspondientes.
- Los usuarios con permiso de lectura y escritura podrán crear tareas dentro del proyecto y organizarlas en carpetas mediante arrastrar y soltar.
- Las tareas podrán reordenarse dentro de una carpeta y trasladarse entre carpetas.
- Una tarea podrá contener subtareas con un máximo de cinco niveles de profundidad: tarea principal y niveles N1, N2, N3 y N4.
- Las tareas y subtareas podrán expandirse y contraerse para mostrar u ocultar su contenido.
- Las tareas y subtareas podrán asignarse a participantes que ya tengan acceso al proyecto. La asignación identifica a la persona responsable y no cambia por sí sola sus permisos de acceso.
- Las tareas internas y sus subtareas no se mostrarán ni modificarán en las vistas normales del módulo Tareas.

### 7.2. Pizarra compartida del proyecto

- Debajo de las carpetas y tareas sueltas de la vista principal existirá una pizarra blanca con apariencia de pizarra física de colegio.
- Los participantes podrán abrir un editor propio de EnOrden, escribir mediante el teclado disponible en su dispositivo y guardar una anotación.
- Al guardar, EnOrden reproducirá una animación que simule la escritura manual sobre la superficie de la pizarra.
- El texto final utilizará una tipografía manuscrita y un tratamiento visual que simule escritura con plumón.
- Las anotaciones podrán utilizarse para recordatorios, observaciones y mensajes breves del equipo.
- Todos los participantes con acceso podrán leer y publicar anotaciones, independientemente de que tengan permiso de lectura o de lectura y escritura para las tareas.
- La pizarra no crea tareas automáticamente.

### 7.3. Chat del proyecto

- Cada proyecto tendrá un chat grupal propio, separado del chat general de EnOrden.
- El chat se abrirá desde un botón flotante «Chat» ubicado en la esquina inferior derecha. La ventana se desplegará junto al botón, como un panel contextual, y podrá cerrarse desde el mismo panel o botón.
- El botón mostrará un indicador circular en su esquina superior derecha cuando haya mensajes nuevos sin revisar; al abrir el chat, el indicador se quitará.
- Todos los participantes con acceso al proyecto podrán leer y enviar mensajes, incluidos quienes tengan permiso de solo lectura.
- El chat conservará su historial dentro del proyecto y no modificará tareas ni otros módulos.
- La interfaz del chat y su editor serán componentes propios de EnOrden.

### 7.4. Prototipo visual y etapas de sincronización

- La primera entrega visual podrá utilizar contenido de demostración para que Vale revise la distribución, los estilos y la jerarquía de los tres espacios.
- El contenido de demostración debe identificarse como tal y no presentarse como información real ni guardada.
- Mientras el almacenamiento siga siendo local, las anotaciones, mensajes y tareas de prueba no representan colaboración sincronizada entre cuentas o dispositivos.
- La sincronización real del chat, la pizarra, las tareas y sus cambios requiere almacenamiento centralizado y autorización del lado del servidor.

## 8. Almacenamiento y evolución técnica

Primera versión:
- Se trabajará inicialmente con almacenamiento local del navegador.
- La implementación local es una etapa de prototipo y no proporciona por sí sola sincronización real entre distintos usuarios/dispositivos.
- No presentar la colaboración entre cuentas como funcionalidad multiusuario real mientras no exista backend compartido.
- Estructurar los datos separando proyectos, participantes, solicitudes, permisos, tareas internas y preferencias individuales, para facilitar una futura migración.

Futuro:
- Se prevé migrar a almacenamiento centralizado, autenticación y autorización del lado del servidor.
- Las notificaciones de vencimiento, la eliminación automática independiente de la sesión y los enlaces de invitación multiusuario requieren servicios/backend para ser fiables en producción.
- La migración futura no debe mezclar los datos de proyectos con las tareas normales.

## 9. Principios de implementación

- Compartido debe permanecer separado del módulo Tareas.
- No crear proyectos desde el editor normal de tareas.
- No mostrar tareas de proyectos en páginas normales.
- No conceder acceso solo por abrir un enlace.
- No asignar usuarios que todavía no tienen acceso.
- No permitir que participantes sin permiso modifiquen permisos o administren solicitudes.
- La eliminación de un proyecto afecta a todos los participantes.
- Las preferencias de orden y anclaje pertenecen a cada usuario.
- Implementar por etapas, validar cada etapa y evitar cambios ajenos al alcance solicitado.
- Todas las modificaciones de desarrollo se realizan en la rama EnOrden-Desarrollo. No publicar en main sin autorización explícita.

## 10. Etapas de desarrollo acordadas

1. Página principal Compartido: listado de proyectos, orden manual, anclaje y navegación; usar datos locales/de prueba.
2. Creación de proyectos desde Compartido y generación de páginas individuales.
3. Plantilla visual común para las páginas de proyecto, con los espacios de tareas, pizarra y chat.
4. Modelo local de datos para proyectos y preferencias, con estructura preparada para migración.
5. Carpetas, tareas internas, subtareas de hasta cinco niveles y asignaciones.
6. Pizarra compartida: editor propio, anotaciones y animación de escritura.
7. Chat del proyecto: interfaz, envío de mensajes e historial.
8. Invitaciones, solicitudes, participantes, permisos y administración.
9. Sincronización multiusuario mediante backend compartido y autorización del lado del servidor.
10. Vencimiento, aviso con un día de anticipación, eliminación automática local y eliminación manual con confirmación.
11. Pruebas de flujos, privacidad y separación respecto de Tareas.

Nota de alcance: las etapas 5 a 8 tendrán limitaciones importantes mientras el almacenamiento sea exclusivamente local. Antes de ofrecerlas como colaboración real entre cuentas, deberán migrarse a backend centralizado con autenticación, permisos y tareas programadas.
