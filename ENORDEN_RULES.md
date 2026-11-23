# EnOrden — reglas generales del proyecto

**Documento rector:** estas reglas se aplican a todo EnOrden y prevalecen sobre las especificaciones particulares de cada módulo.

**Rama de trabajo autorizada:** `EnOrden-Desarrollo`  
**Rama protegida:** `main`

## 1. Flujo de trabajo y control de versiones

- Todo desarrollo, prueba y corrección se realizará primero en la rama `EnOrden-Desarrollo`, que funciona como espacio de pruebas.
- El flujo de trabajo se mantendrá entre los espacios de prueba y la rama de publicación: se desarrollará y validará en `EnOrden-Desarrollo`, sin trabajar directamente sobre `main`.
- Cada vez que Vale autorice y se publique una versión en `main`, la rama `EnOrden-Desarrollo` deberá retomarse desde esa misma versión publicada. Así, el trabajo de prueba continuará sobre el estado más reciente de `main` y se evitarán historiales divergentes, commits huérfanos o cambios exclusivos cuyo origen no esté claro.
- La actualización de `EnOrden-Desarrollo` desde `main` después de una publicación forma parte del cierre de esa publicación. Debe comprobarse que ambas ramas vuelvan a compartir el mismo punto de partida antes de continuar desarrollando.
- `main` es la rama de publicación y permanecerá protegida frente a cambios de desarrollo. No se realizarán cambios directamente en `main`, ni se fusionarán ramas hacia `main`, ni se publicarán cambios definitivos sin una solicitud explícita de Vale.
- Una autorización para modificar o publicar en `main` debe ser expresa y referirse a esa acción. No se interpretará el silencio, una aprobación de otra tarea ni una solicitud de desarrollo como autorización para tocar `main`.
- Antes de escribir cambios, comprobar la rama y revisar el estado relevante del repositorio.
- Mantener los cambios acotados a lo solicitado. No modificar módulos, navegación, cabecera, estilos o infraestructura compartida que no formen parte del alcance autorizado.
- Los Deploy Previews se utilizarán para revisar visualmente los cambios cuando estén disponibles. Nunca afirmar que un despliegue se actualizó o que una prueba pasó sin haberlo verificado.

## 2. Arquitectura modular

- Cada módulo debe ser lo más independiente y autocontenido posible.
- La comunicación entre módulos debe realizarse mediante interfaces o contratos definidos.
- Un cambio interno de un módulo no debe romper ni alterar el comportamiento de otros módulos.
- Los datos, estados y funciones propios de un módulo deben permanecer dentro de su ámbito, salvo las interfaces compartidas acordadas.
- Los cambios en componentes o servicios globales requieren autorización explícita cuando no formen parte directa de la solicitud.

## 3. Documentación obligatoria por módulo

- Cada módulo debe tener un manual propio en la raíz del repositorio, con el formato `NOMBRE_SPEC.md` (por ejemplo, `ENTRETENCION_SPEC.md`).
- El manual debe describir propósito, alcance, funciones, reglas de negocio, datos, dependencias, interfaces con otros módulos, estados y pendientes relevantes.
- Actualizar el manual correspondiente cuando se acuerden o implementen cambios que alteren sus reglas, comportamiento o arquitectura.
- Los manuales particulares deben respetar este documento rector.

## 4. Comentarios y registro de pendientes en el código

- Dejar comentarios en el código cuando ayuden a explicar decisiones importantes, restricciones, riesgos o trabajo pendiente.
- Usar etiquetas consistentes:
  - `TODO:` trabajo pendiente de implementar.
  - `REVISAR:` comportamiento o decisión que requiere comprobación.
  - `BUG:` problema conocido.
  - `MEJORA:` mejora futura que no bloquea el funcionamiento actual.
  - `DECISIÓN:` decisión acordada que conviene preservar.
- Los comentarios deben ser concretos y mantenerse actualizados; eliminar o corregir los que hayan quedado obsoletos.
- Los pendientes importantes también deben figurar en el manual del módulo para que no dependan únicamente de encontrar un comentario en el código.

## 5. Revisión, pruebas y comunicación

- Guardar los cambios en la rama de pruebas y revisar el diff antes de dar una tarea por terminada.
- Comprobar sintaxis y funcionamiento en la medida que permitan las herramientas disponibles.
- Distinguir claramente entre cambios guardados, pruebas ejecutadas, pruebas pendientes y despliegues verificados.
- Informar qué archivos y módulos fueron modificados, qué se comprobó y qué queda pendiente.
- No declarar una función terminada si solo se ha escrito el código y todavía no se ha probado.

## 6. Precedencia y detección de contradicciones

- Este archivo (`ENORDEN_RULES.md`) es la fuente principal de las reglas generales del proyecto.
- Las especificaciones de módulos (`*_SPEC.md`) pueden añadir detalles y reglas particulares, pero no pueden contradecir las reglas generales.
- **Antes de comenzar un trabajo en un módulo, revisar su manual junto con este documento y comprobar si existe alguna contradicción.**
- **Si una regla de un manual de módulo contradice una regla de `ENORDEN_RULES.md`, avisar a Vale de forma explícita, identificar ambos textos y explicar el conflicto antes de implementar cambios afectados por él.**
- No resolver silenciosamente la contradicción ni modificar por cuenta propia la regla general. Detener únicamente la parte del trabajo afectada y solicitar a Vale que defina cómo proceder.
- Si una regla particular parece necesitar una excepción, pedir autorización explícita y documentar la excepción sin borrar ni debilitar la regla general.

## 7. Continuidad entre conversaciones

- Este documento y los manuales de cada módulo son la referencia persistente del proyecto.
- Al continuar EnOrden en un chat nuevo, revisar primero `ENORDEN_RULES.md` y luego el `NOMBRE_SPEC.md` del módulo que se trabajará.
- Si el repositorio o los documentos no están disponibles, informar esa limitación y no asumir que las reglas siguen siendo las mismas de memoria.

## 8. Principio de actualización acumulativa de decisiones

- Las decisiones nuevas deben incorporarse a la documentación cuando complementen o terminen de definir reglas ya existentes.
- Una decisión nueva no reemplaza, elimina ni debilita una regla anterior compatible.
- Al actualizar un documento, conservar las reglas anteriores que sigan vigentes e integrar las nuevas en la sección correspondiente; evitar duplicaciones innecesarias sin perder su contenido.
- Si una decisión nueva contradice una regla anterior, no escoger automáticamente cuál prevalece ni sobrescribir ninguna: identificar el conflicto y solicitar a Vale una definición explícita.
- Las decisiones que todavía no estén suficientemente definidas deben registrarse como pendientes, indicando con claridad qué falta decidir o validar. No completar vacíos mediante suposiciones.
- Antes de dar por terminada una revisión documental, contrastar las decisiones recientes acordadas con los documentos existentes y verificar que estén registradas en el manual correspondiente.


## 9. Arquitectura general y proveedores

- Cada módulo tendrá su propia configuración, proveedores, fuentes de datos, caché y reglas de funcionamiento.
- Cambiar un proveedor en un módulo no debe alterar otros módulos, aunque compartan una fuente.
- Los proveedores deben poder reemplazarse sin reconstruir la interfaz ni la lógica del módulo.
- Los errores de un proveedor no deben detener el funcionamiento de otros módulos.
- Las credenciales de API deben mantenerse protegidas y nunca exponerse en el código público del navegador.
- La arquitectura debe permitir utilizar intermediarios o servicios de servidor para conectar proveedores externos.
- Cada módulo debe conservar la independencia de su configuración, fuentes, proveedores, caché y reglas, respetando las interfaces o contratos compartidos que se hayan definido.

## 10. Fuentes y créditos

- En Configuración, debajo de Ayuda, existirá un desplegable llamado **Fuentes y créditos**.
- Cada nueva base de datos, API o proveedor externo deberá registrarse allí.
- La sección será centralizada: los módulos no tendrán que mantener sus propias copias de las atribuciones.
- Las atribuciones dependerán del proveedor configurado y de la procedencia real de los datos. Si se reemplaza un proveedor, deberán actualizarse las referencias correspondientes.
- Se distinguirán proveedores actuales y fuentes históricas.
- Se conservarán los créditos de fuentes anteriores mientras existan datos que dependan de ellas o sus condiciones exijan mantenerlos.
- No es necesario atribuir individualmente cada pequeño texto de una ficha si una atribución conjunta representa correctamente las fuentes involucradas.
- Las categorías y su visibilidad serán:
  - **Visible:** Proveedores.
  - **Visible:** Fuentes originales.
  - **Visible:** Atribuciones.
  - **Visible:** Licencias y condiciones (parte informativa).
  - **Interna:** Estado de conexión.
  - **Interna:** Países y regiones configurados.
  - **Interna:** Fechas de consulta.
  - **Interna:** Procedencia detallada de cada dato.
  - **Interna:** Historial de cambios.
  - **Nunca visibles:** Credenciales API.

## 11. Almacenamiento mínimo necesario

- EnOrden almacenará únicamente los datos necesarios para el funcionamiento de sus módulos, la experiencia del usuario, la administración de caché, la trazabilidad de las fuentes y la conservación de información que el usuario haya decidido guardar.
- Cada módulo determinará los datos concretos que necesita conservar, respetando esta regla general.
- No se conservarán copias duplicadas, respuestas completas de proveedores cuando solo se utiliza una parte, historiales innecesarios ni datos sin utilidad funcional.
- Las reglas particulares de almacenamiento y caché de cada módulo se documentarán en su SPEC correspondiente.

## 12. Actualizaciones y usuarios de prueba

- Las actualizaciones importantes de EnOrden se probarán primero con usuarios específicos de prueba.
- Los usuarios de prueba recibirán notificaciones manuales indicando qué deben probar.
- Después de validar la actualización, los usuarios generales recibirán un aviso con al menos 24 horas de anticipación.
- La publicación general ocurrirá después de la aprobación.
- El sistema deberá permitir activar una versión para determinados usuarios sin que los demás la reciban accidentalmente.
- Estas reglas se aplican a futuras actualizaciones importantes, no solamente a cambios de proveedores.
- El desarrollo del sistema de usuarios y grupos de publicación gradual queda pendiente.

## 13. Preparación comercial y condiciones legales

- EnOrden no será comercial por ahora.
- La arquitectura se preparará técnicamente para una futura versión comercial.
- Las condiciones, licencias, permisos, costos y contratos de cada proveedor deberán revisarse antes de monetizar.
- Tener una API gratuita no significa tener autorización comercial.
- La preparación técnica y la autorización legal serán estados independientes.
- Se revisarán los derechos de imágenes, marcas y enlaces antes de incorporar recursos gráficos o funcionalidades que los requieran.

## 14. Sistema general de ventanas

### 14.1. Principio base y alcance

- Las categorías de ventanas definidas en esta sección serán la **base de diseño y comportamiento para la creación de todas las ventanas de EnOrden**, tanto las existentes que se adapten como las nuevas que se desarrollen en el futuro.
- Estas categorías no son plantillas rígidas ni limitan las características de cada ventana. Cada ventana conservará y podrá incorporar los campos, controles, textos, contenidos, opciones, botones, configuraciones y acciones que requiera su función particular.
- La ventana particular deberá partir de la estructura base de la categoría que le corresponda y combinarla con sus necesidades específicas. Por tanto, cumplir la base no significa reducir ni reemplazar su funcionalidad propia.
- Las categorías base son: **ventana de guardado**, **ventana de confirmación**, **ventana de precaución** y **aviso informativo**.
- Antes de crear o modificar una ventana, identificar su categoría y aplicar sus reglas base. Si una ventana requiere una excepción a esas reglas, debe quedar expresamente definida y autorizada por Vale antes de implementarse.
- Estas reglas se aplican transversalmente a todos los módulos, excepto las ventanas y diálogos de **Tareas → Compartidos**, que quedan excluidos de cualquier aplicación o modificación derivada de este sistema general, salvo autorización expresa posterior de Vale.
- Ninguna ventana debe cerrarse ni ejecutar acciones por un clic o toque fuera de sus límites. La interacción fuera de la ventana no tendrá ningún efecto.

### 14.2. Elementos comunes de diseño

- Los títulos estarán centrados.
- La X de cierre estará dentro de un círculo, ubicada en la esquina superior derecha y sobresaliendo ligeramente del borde de la ventana.
- La X tendrá el mismo efecto que la acción secundaria de la categoría correspondiente: en ventanas de guardado y precaución actuará como «Cancelar»; en ventanas de confirmación y avisos informativos tendrá el mismo efecto que su acción de cierre o aceptación definida para ese aviso.
- Los botones deberán conservar márgenes suficientes, separación entre sí y espacio vertical y horizontal. No podrán solaparse con otros elementos ni quedar pegados a los bordes.
- El texto de cada botón deberá permanecer correctamente alineado y completamente dentro de sus límites, sin desbordarse ni solaparse.
- La distribución de los botones y del contenido deberá adaptarse al espacio disponible, manteniendo legibilidad y márgenes adecuados.

### 14.3. Ventana de confirmación

**Estructura base**
- Título centrado.
- X circular en la esquina superior derecha, sobresaliendo ligeramente del borde.
- Un botón principal al final de la ventana, centrado horizontalmente y con espacio suficiente alrededor.
- El texto predeterminado del botón será «Entendido», pero podrá cambiarse por el texto que requiera la función particular.

**Comportamiento base**
- Clic o toque fuera de la ventana: no produce ningún efecto.
- X: ejecuta el mismo efecto que el botón principal de confirmación.
- Botón principal: ejecuta la acción de confirmación o cierre definida para el caso concreto.

**Configuración particular**
- La ventana podrá tener un título, contenido, textos y acción de confirmación propios. También podrá mostrar información, fechas, datos o resultados adicionales, siempre conservando la estructura base de esta categoría.

### 14.4. Ventana de guardado

**Estructura base**
- Título centrado.
- X circular en la esquina superior derecha, sobresaliendo ligeramente del borde.
- Dos botones al final de la ventana: una acción secundaria a un lado y la acción principal de guardado al otro, con una separación pequeña pero visible y márgenes suficientes en ambos extremos.
- Los textos predeterminados serán «Cancelar» y «Guardar», pero podrán adaptarse a la función particular (por ejemplo, «Guardar cambios»).

**Comportamiento base**
- Clic o toque fuera de la ventana: no produce ningún efecto.
- Acción principal: ejecuta el guardado definido para el caso concreto. El destino, la validación y el resultado se especificarán según la funcionalidad correspondiente.
- Acción secundaria o X: no guarda los datos y abre la ventana de precaución correspondiente. Los datos introducidos se conservan temporalmente mientras el usuario decide si continúa editando o sale sin guardar.

**Configuración particular**
- La ventana conservará todos los campos, controles, opciones, botones adicionales, previsualizaciones y configuraciones que requiera su función. La estructura base no reemplaza ni simplifica el formulario particular.
- Los textos de las acciones podrán ser específicos, siempre que se mantenga clara la diferencia entre guardar y cancelar.

### 14.5. Ventana de precaución

**Estructura base**
- Título centrado. El título predeterminado será «¿Quieres salir sin guardar?», pero podrá personalizarse según la acción que se esté confirmando.
- Texto explicativo debajo del título, en tamaño pequeño y color gris. El texto predeterminado será «Si sales sin guardar de esta ventana perderás los datos ingresados», pero podrá adaptarse al contexto.
- X circular en la esquina superior derecha, sobresaliendo ligeramente del borde.
- Dos botones al final, distribuidos como en la ventana de guardado: con separación visible entre ellos y márgenes suficientes a ambos lados.
- La acción destructiva o que implique descartar, eliminar o abandonar tendrá fondo rojo.
- Los textos predeterminados serán «Salir sin guardar» y «Cancelar», pero podrán personalizarse según el caso.

**Comportamiento base**
- Clic o toque fuera de la ventana: no produce ningún efecto.
- Acción secundaria o X: cierra únicamente la ventana de precaución y regresa a la ventana anterior, conservando los datos y el estado temporal para que el usuario pueda continuar.
- Acción destructiva: ejecuta únicamente la acción confirmada para ese caso. Si se trata de abandonar una edición, descarta los datos introducidos y cierra las ventanas involucradas en ese flujo, sin guardar ni afectar información ajena a esa edición. Si se trata de eliminar un elemento existente, elimina exclusivamente el elemento identificado y confirmado.

**Configuración particular**
- El título, texto explicativo, botones y acción destructiva se definirán para cada situación. Por ejemplo, una eliminación puede usar un título como «¿Quieres eliminar este evento?» y mostrar claramente qué elemento se eliminará.
- La ventana debe identificar con claridad qué acción se realizará y sobre qué elemento, cuando corresponda.

### 14.6. Aviso informativo

- El aviso informativo es una categoría independiente; no es una variante de la ventana de confirmación, guardado o precaución.
- Mantendrá como elementos comunes el título centrado cuando corresponda, la X circular en la esquina superior derecha sobresaliendo ligeramente del borde y la protección contra cierres por clic o toque fuera.
- Su contenido, distribución, textos, botones y acciones se configurarán según la información que deba comunicar. No se le impondrá la estructura de dos botones de una ventana de guardado o precaución.
- La X cerrará el aviso. Cualquier botón adicional ejecutará la acción informativa o complementaria definida para ese caso.

### 14.7. Aplicación, revisión y excepciones

- Estas cuatro categorías son la base para todas las ventanas existentes y futuras de EnOrden. La adaptación debe respetar la función particular de cada ventana y no eliminar opciones o comportamientos ya definidos.
- Las ventanas existentes se adaptarán según la clasificación y el alcance acordados con Vale. No se considerará autorizada una modificación solo porque una ventana aparezca en un inventario técnico: deben respetarse las inclusiones, exclusiones y pendientes definidos.
- Las ventanas de **Tareas → Compartidos** quedan expresamente fuera del sistema general de ventanas y no deben modificarse por cambios globales en estilos, componentes o comportamientos.
- Los selectores y controles auxiliares (por ejemplo, ruedas para elegir hora o cantidades) no se convertirán automáticamente en ventanas de guardado, confirmación o precaución; conservarán su naturaleza de selector, salvo que Vale defina otra cosa.
- Revisar los usos de los diálogos nativos del navegador (`alert()`, `confirm()` y `prompt()`). Cuando correspondan a confirmaciones, advertencias, validaciones o avisos propios de EnOrden, reemplazarlos por ventanas de la aplicación de la categoría adecuada, preservando el flujo y la función original. Antes de reemplazarlos, inspeccionar cada uso para no alterar el comportamiento ni intervenir flujos fuera del alcance autorizado.
- Toda ventana nueva deberá clasificarse antes de implementarse y partir de la estructura base correspondiente, manteniendo sus características particulares.
## 15. Selectores de hora personalizados

### 15.1. Reloj Rueda (selector activo)

- **Reloj Rueda** es el selector de hora oficial y activo de EnOrden.
- Todo botón, campo o control que permita seleccionar o modificar una hora deberá utilizar el componente compartido **Reloj Rueda**. No se utilizarán selectores nativos del navegador o del sistema operativo (como `input type="time"`, `showPicker()` u otros equivalentes) para la interacción de selección de hora.
- El selector tendrá dos ruedas independientes: una para las horas y otra para los minutos. Las horas se mostrarán en formato de 24 horas, desde 00 hasta 23, y los minutos desde 00 hasta 59, permitiendo elegir cualquier minuto individual.
- Ambas ruedas tendrán un borde negro metálico. Su diseño, colores, tipografía, dimensiones, desplazamiento, estados seleccionados y comportamiento serán definidos por EnOrden y deberán mantenerse consistentes en teléfonos, tabletas y computadores, independientemente del sistema operativo, navegador, fabricante, tamaño de pantalla o tema elegido.
- Se mostrará una vista previa de la hora completa durante la selección y las acciones «Cancelar» y «Aceptar». Cancelar conservará el valor anterior sin aplicar cambios; Aceptar actualizará el campo asociado y ejecutará la actualización de vista previa o lógica correspondiente.
- El componente recibirá el valor inicial del campo que lo invoca y deberá funcionar tanto al crear como al editar información.
- Cualquier nueva funcionalidad que incorpore selección de hora deberá reutilizar este componente. No crear selectores de hora independientes ni excepciones por módulo sin autorización expresa de Vale.
- Esta regla se aplica a todos los módulos y a cualquier flujo futuro que permita elegir una hora. Los campos de hora almacenados internamente pueden conservar el formato de datos que requiera su función; la regla define la interfaz de selección.

### 15.2. Reloj Analógico (componente conservado y oculto)

- El componente anteriormente denominado selector de hora analógico pasa a llamarse **Reloj Analógico**.
- El Reloj Analógico queda oculto y fuera de uso: no será invocado por botones de hora actuales ni por nuevas funcionalidades mientras permanezca pendiente de revisión.
- Se conservará su implementación y sus reglas visuales como referencia para retomarlo posteriormente. Su ocultamiento no implica eliminarlo ni rediseñarlo.
- **Pendiente:** revisar y resolver la dificultad de selección individual de minutos en el Reloj Analógico. Solo después de esa revisión Vale decidirá si vuelve a habilitarse o si requiere otros ajustes.
- Las reglas de apariencia que se conservan para el Reloj Analógico son: círculo exterior vacío con acabado negro metálico; círculo interior con color base `var(--surface2)`, contrastes, degradados y sombras; borde exterior negro `#000`; paleta fija independiente del tema y del dispositivo.


## 16. Orden fijo de módulos y navegación

- El orden de los módulos será fijo, idéntico para todos los usuarios y dispositivos. No se permitirá reordenarlos mediante arrastre, pulsación prolongada, preferencias personales ni ningún otro mecanismo.
- La barra inferior de navegación tendrá este orden exacto:
  1. Inicio
  2. Calendario
  3. Tareas
  4. Listas
  5. Rutinas
  6. Finanzas
  7. Notas
  8. Entretención
  9. Mapa
  10. Chat
- La sección «Mis módulos» de la página Inicio utilizará el mismo orden, omitiendo únicamente «Inicio»:
  1. Calendario
  2. Tareas
  3. Listas
  4. Rutinas
  5. Finanzas
  6. Notas
  7. Entretención
  8. Mapa
  9. Chat
- El orden definido en esta sección será la única fuente de verdad para ambas ubicaciones. No se guardarán ni aplicarán órdenes personalizados por dispositivo o usuario.
- Todo módulo nuevo deberá incorporarse a esta regla únicamente cuando Vale defina su posición. No deberá alterar ni desplazar el orden existente por iniciativa propia.
