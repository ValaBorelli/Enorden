# EnOrden — especificación funcional de Tareas

Estado: reglas funcionales acordadas para las vistas y flujos definidos en noviembre de 2026. Los puntos que requieren una definición adicional están identificados como pendientes.
Rama de desarrollo: EnOrden-Desarrollo.
Este documento regula las tareas normales de EnOrden. No regula los proyectos del módulo Compartido ni el catálogo del módulo Entretención.

## 1. Alcance y separación

- El módulo Tareas administra tareas personales y familiares, sus subtareas, programación, vistas y plantillas.
- Las tareas personales y familiares pertenecen al sistema de tareas normales.
- Los proyectos y las tareas internas de proyectos pertenecen exclusivamente al módulo Compartido y no deben mezclarse con las tareas normales.
- Los cambios internos de Tareas no deben modificar otros módulos ni componentes globales fuera del alcance autorizado.
- Las tareas terminadas se conservan durante un máximo de 7 días y luego se eliminan del sistema.

## 2. Jerarquía de tareas y subtareas

- La estructura admite una tarea principal, subtareas de primer nivel (N1) y subtareas de segundo nivel (N2).
- Una N2 solo puede existir dentro de una N1.
- Las reglas de asignación y finalización deben respetar esta jerarquía.
- Al marcar como terminada una tarea principal, se marcan como terminadas sus N1 y N2.
- Al marcar como terminada una N1, se marcan como terminadas sus N2.
- Al marcar una N2, solo se marca esa N2.
- Marcar una tarea o subtarea como terminada no debe contraer automáticamente su contenido: la jerarquía debe permanecer desplegada.
- Las secciones y los grupos desplegables solo se abren o cierran mediante su flecha o encabezado, no como efecto secundario de marcar casillas.

## 3. Creación y edición de tareas

- La creación de tareas normales se inicia exclusivamente desde el botón «+».
- La ventana de creación aparece centrada y se titula exactamente «Nueva tarea».
- La ventana incluye el nombre de la tarea, controles para crear subtareas y las acciones «Guardar», «Cancelar» y «X».
- Pulsar fuera de la ventana no la cierra.
- Para crear una tarea se debe elegir obligatoriamente el destino mediante el selector «¿Dónde se verá?», con las opciones «Tareas Familia» y «Tareas Personales».
- «Tareas Personales» es el destino seleccionado por defecto.
- La asignación a otros usuarios, cuando corresponda, se realiza después de crear la tarea.
- La edición debe conservar la jerarquía y las configuraciones existentes, permitiendo modificar las opciones disponibles de la tarea.
- La programación es opcional y aparece desmarcada por defecto. Si se activa, la fecha es obligatoria.
- Una tarea programada genera un evento de prioridad alta en el Calendario, identificado en color naranja.
- El evento del Calendario debe poder editarse y mantenerse sincronizado con los cambios de la tarea.

## 4. Asignaciones y responsables

- Las tareas personales y sus subtareas N1/N2 no se asignan a otros usuarios.
- Las tareas familiares y sus subtareas sí admiten asignación.
- La tarea principal y las N1 pueden asignarse o reasignarse.
- Una N2 hereda el responsable de su N1; si corresponde, hereda el responsable de la tarea principal. No tiene asignación individual independiente en la versión definida.
- La asignación de una N1 afecta únicamente a esa N1 y a sus N2. No cambia el responsable de la tarea principal ni el de las demás N1.
- Cambiar el responsable de la tarea principal no debe sobrescribir una asignación propia ya definida en una N1.
- La opción «Sin asignar» elimina la asignación del nivel seleccionado.
- El responsable debe mostrarse junto a la flecha de despliegue, más cerca de esta que del nombre de la tarea, y también en N1 y N2 mostrando la asignación propia o heredada.
- La asignación individual de N2 queda pendiente y no debe implementarse como si estuviera aprobada.

## 5. Navegación y vistas

### 5.1 Inicio y accesos

- Inicio presenta seis tarjetas principales: «Mi día», «Todas», «Pendientes», «Familia», «Personales» y «Asignadas».
- Las tarjetas «Familia» y «Personales» abren sus respectivas páginas.
- «Todas» se abre exclusivamente desde su tarjeta de Inicio; no debe añadirse un botón «Todas» al panel lateral.
- «Asignadas» se accede desde su tarjeta de Inicio y no debe figurar como acceso del panel lateral.
- «Mis Plantillas» es un acceso independiente que navega a la página «Mis plantillas de tareas».
- El panel lateral conserva su tamaño fijo; los cambios de navegación no deben alterar sus dimensiones.

### 5.2 Familia y Personales

- «Familia» muestra las tareas familiares.
- «Personales» muestra las tareas personales.
- Estas vistas conservan las tarjetas, subtareas y controles propios de las tareas normales.
- La responsabilidad solo se ofrece en tareas familiares; las tareas personales no admiten asignaciones a otros usuarios.

### 5.3 Todas

- La vista «Todas» reúne las tareas normales en este orden:
  1. «Tareas Personales».
  2. «Tareas Familia».
  3. «Tareas Programadas».
  4. «Terminadas».
- «Tareas Programadas» contiene las subsecciones «Personales» y «Familiares».
- Al desplegar «Tareas Programadas», sus dos subsecciones se muestran desplegadas.
- Las secciones y subsecciones se controlan mediante sus flechas o encabezados.
- La vista mantiene la apariencia y los controles habituales de Familia y Personales, sin convertir las tareas programadas en una categoría ajena al sistema de tareas.

### 5.4 Mi día

- Muestra tareas pendientes sin fecha o programadas para el día actual.
- No muestra tareas programadas para fechas futuras, tareas con fecha pasada ni tareas completamente terminadas.

### 5.5 Asignadas

- Muestra tareas pendientes asignadas al usuario y aplica el mismo filtro temporal de «Mi día»: sin fecha o programadas para hoy; excluye futuras, pasadas y completamente terminadas.
- Si la tarea principal está asignada al usuario, se muestra la tarea completa con todas sus subtareas.
- Si solo una N1 está asignada al usuario, se muestra la tarea principal como contenedor y únicamente esa N1 con sus N2 enlazadas. Se ocultan las demás N1 y sus contenidos.
- Cuando solo una N1 está asignada, no se muestra la casilla de finalización de la tarea principal, para impedir que el usuario complete accidentalmente toda la tarea.
- El contador de «Asignadas» debe incluir las N1 asignadas, aunque la tarea principal no esté asignada.
- La finalización desde esta vista debe respetar los niveles realmente asignados al usuario.

### 5.6 Pendientes

- La vista reúne las tareas personales pendientes y las tareas asignadas pendientes que correspondan al usuario.
- Incluye una sección desplegable «Tareas programadas».
- En «Tareas programadas» se muestran únicamente tareas pendientes cuya fecha programada todavía no haya pasado.
- Una tarea programada desaparece de esa sección cuando se completa o cuando vence su fecha, incluso si estaba programada para varios meses después.
- Las tareas programadas para hoy se muestran en la vista de tareas correspondiente al día actual, no como próximos pendientes futuros.
- Las tareas con subtareas deben poder desplegarse.
- Deben mostrarse las subtareas pendientes reales de toda la jerarquía, incluso si su N1 o su tarea principal ya está terminada.
- Las casillas y los contadores deben considerar esas subtareas pendientes reales, sin ocultarlas por el estado terminado de un nivel superior.

### 5.7 Tarjeta de próximos eventos

- El número mostrado en la tarjeta de Inicio debe coincidir exactamente con el número de «Próximos eventos» del Calendario.
- Debe utilizar los mismos filtros y límites del Calendario, incluyendo el horizonte de los próximos 7 días y el tratamiento de eventos repetidos.

## 6. Plantillas de tareas

- Las tareas pueden guardarse como plantillas para volver a generar tareas con las mismas configuraciones y la misma jerarquía de subtareas.
- Las plantillas se almacenan en una página independiente llamada exactamente «Mis plantillas de tareas», accesible mediante el botón «Mis Plantillas».
- La plantilla conserva el nombre, las configuraciones, el destino/categoría Personal o Familia y la jerarquía completa de subtareas N1/N2.
- Guardar o editar una plantilla no debe modificar la tarea original.
- Reutilizar una plantilla crea una tarea nueva; no reactiva ni modifica la tarea o plantilla de origen.
- Todas las opciones de una plantilla deben poder modificarse al editarla.
- La página «Mis plantillas de tareas» muestra las plantillas guardadas y estas aparecen contraídas por defecto.
- Cada plantilla conserva la acción «Eliminar», que solicita confirmación antes de borrarla.
- Al mantener pulsada una plantilla durante más de 5 segundos, se pregunta si se desea reutilizarla:
  - **Sí:** abre el flujo de creación/edición de una tarea nueva basada en la plantilla. Guardar crea una tarea nueva y conserva intacta la plantilla original.
  - **No:** abre la edición de la plantilla. Debe avisarse que esta acción no crea una tarea; guardar modifica únicamente la plantilla.
- Una tarea programada guardada como plantilla conserva la indicación de que originalmente tenía programación, pero la plantilla no conserva una fecha activa.
- Al reutilizar una plantilla que proviene de una tarea programada, la nueva tarea queda sin programación por defecto y se informa al usuario que puede volver a programarla.
- En la creación y edición de tareas existe la posibilidad de guardar también la configuración como plantilla. El control exacto debe respetar las acciones acordadas «Guardar Plantilla» y «Guardar también como plantilla», sin cambiar la creación normal de la tarea ni alterar su original.
- Mantener pulsada una tarea normal debe ofrecer la acción «Guardar plantilla».

## 7. Duración, vencimiento y eliminación

- Una tarea terminada permanece disponible durante un máximo de 7 días.
- Cumplido ese plazo, se elimina del sistema.
- Esta eliminación de tareas terminadas es independiente del guardado de plantillas: una plantilla guardada permanece disponible según las reglas de plantillas y no debe desaparecer por la eliminación de la tarea original.
- No se ha definido aquí una duración automática para tareas pendientes no programadas; no debe aplicarse por analogía el plazo de 7 días de las tareas terminadas.

## 8. Pendientes de definición o validación

- **PENDIENTE:** decidir si las N2 podrán tener un responsable individual independiente de la N1. Hasta entonces, las N2 heredan responsable.
- **PENDIENTE DE PRECISIÓN DE INTERFAZ:** consolidar la presentación de los controles «Guardar Plantilla» y «Guardar también como plantilla» en las ventanas de creación y edición, preservando ambas decisiones registradas sin duplicar acciones ni descartar una de ellas por suposición.
- **PENDIENTE DE VALIDACIÓN:** comprobar que la tarjeta de próximos eventos y el Calendario comparten exactamente los mismos filtros, límites y tratamiento de repeticiones.
- **PENDIENTE DE VALIDACIÓN:** probar que las tareas programadas, sus eventos del Calendario y los cambios de fecha se mantienen sincronizados.
- **PENDIENTE DE VALIDACIÓN:** probar los contadores y la visibilidad de subtareas pendientes cuando una tarea principal o una N1 ya está terminada.
- Los detalles no definidos en este documento deben consultarse antes de implementar un comportamiento nuevo; no se deben completar mediante supuestos.

## 9. Principios de implementación

- Mantener separadas las tareas personales, familiares y los proyectos de Compartido.
- Respetar la jerarquía tarea principal → N1 → N2.
- No asignar tareas personales ni permitir asignación individual de N2 mientras siga pendiente esa decisión.
- No mostrar en una vista tareas que sus filtros excluyen.
- No cambiar el estado de una jerarquía por una acción sobre un nivel distinto de los definidos en estas reglas.
- Conservar las plantillas independientemente de la tarea que las originó.
- Implementar y probar los flujos por etapas, sin modificar otros módulos, la navegación global ni componentes compartidos fuera del alcance autorizado.
- Todo desarrollo se realiza en la rama EnOrden-Desarrollo. No publicar ni fusionar hacia main sin autorización explícita.
