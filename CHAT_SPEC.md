# Módulo Chat — EnOrden

Documento de trabajo para definir e implementar el módulo Chat.

## Alcance

- Este documento inicia el trabajo del módulo Chat.
- La funcionalidad, experiencia visual, reglas de privacidad, participantes, notificaciones y persistencia se definirán junto a Vale antes de implementar cambios.
- El trabajo de este módulo se mantendrá aislado: no modificar otros módulos, navegación, encabezado, estilos globales ni componentes compartidos, salvo autorización explícita.
- Todo cambio se realizará en la rama `EnOrden-Desarrollo` y se revisará mediante su Pull Request y Netlify Deploy Preview.
- No fusionar ni publicar en `main` sin autorización expresa de Vale.

## Requisitos aprobados

### 1. Estructura general

1. El Módulo Chat será un módulo independiente dentro de EnOrden.
2. Contará con un chat principal denominado **Familia**.
3. También permitirá generar chats temporales asociados a cada nota creada.
4. El chat Familia deberá mantenerse siempre entre los primeros elementos de la lista de chats, con prioridad sobre los chats temporales.

### 2. Chat Familia

5. El chat Familia estará disponible para todos los integrantes de la familia.
6. Todos los integrantes podrán participar y visualizar las conversaciones.
7. Permitirá enviar y recibir mensajes de texto y mensajes de audio.
8. El chat Familia será un espacio permanente de conversación, pero su contenido tendrá una duración máxima de siete días por ciclo.
9. El primer mensaje enviado cuando no exista un ciclo activo iniciará automáticamente un nuevo ciclo de siete días.
10. Todos los mensajes enviados durante ese ciclo pertenecerán al mismo período de vencimiento, independientemente de la fecha u hora en que hayan sido enviados.
11. El vencimiento se calculará desde el inicio del ciclo, no desde la fecha individual de cada mensaje.
12. Al cumplirse los siete días, se eliminará automáticamente todo el contenido del ciclo: mensajes de texto, audios y demás contenido asociado a las conversaciones.
13. El chat Familia no se eliminará ni se desactivará después del vencimiento. Permanecerá disponible para recibir nuevos mensajes.
14. Una vez vencido un ciclo, el chat quedará sin contenido hasta que un integrante envíe un nuevo mensaje.
15. El primer mensaje enviado después del vencimiento iniciará un nuevo ciclo de siete días.
16. Si nadie envía mensajes después del vencimiento, el chat permanecerá vacío y no comenzará un nuevo ciclo hasta que alguien vuelva a escribir.
17. Veinticuatro horas antes del vencimiento de cada ciclo, todos los integrantes de la familia recibirán una notificación informando que el contenido de la conversación será eliminado.
18. La notificación deberá enviarse una sola vez por ciclo a cada integrante.

### 3. Aviso de funcionamiento del Chat Familia

19. La primera vez que un usuario intente enviar un mensaje en el chat Familia, se mostrará una ventana informativa.
20. La ventana explicará, mediante un texto breve y sencillo, que las conversaciones se eliminan automáticamente cada siete días.
21. El aviso se mostrará una sola vez por usuario y no deberá aparecer nuevamente en cada ciclo.
22. El usuario deberá poder cerrar el aviso y continuar con el envío de su mensaje.

**Texto informativo aprobado como propuesta:**

> ¡Bienvenidos al chat Familia!
>
> Este chat renueva sus conversaciones cada 7 días. Al finalizar cada período, todos los mensajes y audios se eliminan automáticamente para dar espacio a nuevas conversaciones.
>
> ¡Disfruten de este espacio para mantenerse conectados!

### 4. Chats asociados a Notas

23. Cada vez que un integrante cree una nota, se generará automáticamente un chat temporal asociado exclusivamente a esa nota.
24. Todos los integrantes de la familia podrán visualizar y participar en el chat de cada nota.
25. Los integrantes podrán comentar y responder mediante mensajes de texto.
26. Los chats asociados a notas no permitirán enviar audios ni otros formatos de mensajes.
27. La duración máxima de cada chat será de 24 horas.
28. El plazo de 24 horas comenzará a contarse desde el momento exacto en que se cree la nota, independientemente de cuándo se envíe el primer mensaje.
29. Al cumplirse las 24 horas, el chat asociado a la nota se eliminará automáticamente por completo.
30. La eliminación incluirá todos los mensajes, comentarios y contenido almacenado dentro del chat.
31. El chat desaparecerá de la lista de conversaciones y dejará de existir como espacio de comunicación.
32. El vencimiento y eliminación se ejecutarán aunque nadie haya participado o enviado mensajes en el chat.

### 5. Reglas de independencia y funcionamiento

33. El Chat Familia y los chats asociados a Notas tendrán temporizadores y ciclos de vencimiento independientes.
34. El vencimiento de un chat no deberá afectar el funcionamiento ni el contenido de los demás chats.
35. Las reglas de duración, eliminación y participación deberán cumplirse automáticamente, sin requerir intervención manual de los usuarios.


---

## Primera implementación — alcance de la versión 1

Esta sección define el alcance de la primera versión funcional sin eliminar ni reinterpretar los requisitos generales aprobados anteriormente.

- La versión 1 permitirá enviar mensajes de texto.
- La versión 1 permitirá enviar transcripciones como contenido de texto mediante el micrófono integrado en el compositor del Chat Familia.
- El compositor de Chat Familia mantendrá siempre visibles el campo de escritura, el botón de micrófono y el botón de envío. No habrá que cambiar entre modos Texto y Transcripción.
- El usuario podrá escribir, dictar o combinar ambas formas en un mismo mensaje. El texto reconocido se incorporará al mismo campo de escritura y podrá editarse antes del envío.
- Los chats asociados a notas mantendrán un compositor exclusivamente de texto, sin botón de micrófono.
- El reconocimiento de voz utilizará la API disponible en el navegador (`SpeechRecognition` o `webkitSpeechRecognition`), sin integrar ni contratar un proveedor externo.
- El idioma del reconocimiento se determinará automáticamente a partir del idioma seleccionado en EnOrden: español (`es-CL`), inglés (`en-US`) o portugués (`pt-BR`).
- El texto reconocido se insertará en el campo de escritura y podrá editarse antes de enviarlo.
- La disponibilidad del reconocimiento y de cada idioma dependerá del navegador y del dispositivo. Si no está disponible, EnOrden informará al usuario.
- EnOrden no guardará ni enviará archivos de audio como mensajes; el audio se utiliza únicamente durante el reconocimiento de voz proporcionado por el navegador.
- La grabación y el envío de mensajes de audio originales quedan reservados para una versión futura.
- Las transcripciones se someterán a las mismas reglas de duración y eliminación que los mensajes de texto del chat correspondiente.

## Separación técnica obligatoria

El módulo Chat se desarrollará manteniendo tres capas independientes:

1. **Interfaz:** pantallas, lista de conversaciones, vista de mensajes, compositor, estados vacíos, controles y presentación visual. No contendrá reglas de persistencia ni decisiones propias del proveedor de almacenamiento.
2. **Lógica del chat:** ciclos, vencimientos, creación y eliminación de conversaciones temporales, validaciones, respuestas, avisos y comportamiento de mensajes. No dependerá de una tecnología de servidor específica.
3. **Conexión de datos:** adaptador responsable de leer y guardar mensajes y metadatos. En la etapa actual utilizará almacenamiento local de prueba. Más adelante podrá sustituirse por la conexión al servidor que se elija, sin reconstruir la interfaz ni reescribir las reglas de negocio.

## Limitaciones del prototipo local

- Los datos guardados localmente solo están disponibles en el dispositivo y navegador donde se generaron.
- El prototipo no representa mensajería multiusuario ni sincronización en tiempo real.
- Los mensajes enviados en esta etapa se muestran como enviados por el usuario local.
- Los avisos de vencimiento son una simulación local; las notificaciones fiables para todos los integrantes requerirán una conexión de servidor y un mecanismo de notificaciones.
- El cumplimiento de vencimientos en segundo plano y entre dispositivos deberá garantizarse en la futura capa de servidor.


---

## Forma de trabajo, estado actual y próximos pasos

Esta sección registra cómo continuaremos desarrollando el módulo Chat, qué está construido, qué falta y qué puede avanzarse sin tomar decisiones prematuras sobre servicios externos. No reemplaza ni modifica los 35 requisitos aprobados ni las reglas de la primera versión.

### 1. Método de trabajo obligatorio

- El desarrollo se realizará de forma incremental, en etapas pequeñas y revisables. Antes de cada cambio se identificará qué parte del módulo se modificará y se mantendrá el alcance limitado al módulo Chat independiente.
- Se conservará la separación de tres capas definida en este documento: **interfaz**, **lógica del chat** y **conexión de datos**. Cada cambio deberá respetar esa separación.
- La interfaz no deberá contener reglas de negocio ni depender directamente de un proveedor de almacenamiento. La lógica deberá poder probarse sin depender de un proveedor concreto. La conexión de datos deberá poder sustituirse sin reconstruir la interfaz ni reescribir las reglas del chat.
- Mientras no se elija expresamente una solución definitiva de servidor, base de datos o sincronización, se trabajará con adaptadores y datos locales de prueba. No se asumirá ni seleccionará un proveedor por iniciativa propia.
- Las propuestas de arquitectura, almacenamiento, cuentas, permisos o proveedores se tratarán como opciones de análisis hasta que Vale las apruebe explícitamente. Conversar sobre una alternativa no equivale a elegirla.
- Cada etapa deberá incluir revisión del código y de sus diferencias, comprobaciones técnicas y, cuando corresponda, revisión visual mediante Netlify Deploy Preview. Se informará con claridad qué se implementó, qué se probó realmente y qué sigue pendiente. No se declarará una función terminada si solo se escribió el código.
- Los cambios se realizarán en `EnOrden-Desarrollo`. Se utilizará el Pull Request y su Deploy Preview para revisión. No se fusionará ni publicará en `main` sin autorización expresa de Vale.
- No se modificarán otros módulos, navegación, cabecera, estilos globales, componentes compartidos ni infraestructura ajena al Chat, salvo autorización expresa.
- Las decisiones aprobadas se incorporarán acumulativamente a este documento. Las dudas y alternativas no resueltas se registrarán como pendientes, sin convertirlas en reglas.

### 2. Estado de la implementación actual

La primera estructura funcional del módulo Chat está construida como **prototipo local**. Su estado debe entenderse así:

**Interfaz**
- Existe una lista de conversaciones con Familia en primer lugar.
- Existe una vista de conversación, presentación de mensajes, respuestas y compositor.
- El Chat Familia muestra permanentemente el campo de escritura, el botón de micrófono y el botón de envío en un único compositor.
- El micrófono inicia o detiene el reconocimiento de voz; el idioma se toma de la configuración de EnOrden (es-CL, en-US o pt-BR) y el resultado se incorpora al mismo campo editable.
- El usuario puede escribir y dictar dentro del mismo mensaje, sin cambiar de modo. Los chats asociados a notas siguen siendo exclusivamente de texto.
- Los chats asociados a notas mantienen el formato de texto, de acuerdo con sus reglas aprobadas.
- La interfaz contempla conversaciones temporales asociadas a notas.
- Se incorporó una presentación adaptable a pantallas pequeñas.
- La interfaz y las capas están separadas lógicamente dentro del archivo actual; todavía no están divididas físicamente en archivos independientes.

**Lógica**
- Está implementado localmente el inicio y vencimiento del ciclo de siete días de Familia.
- Familia permanece en la lista y queda vacía al vencer su ciclo.
- Está implementada la creación lógica de conversaciones temporales para las notas y su vencimiento a las 24 horas desde la creación de cada nota.
- Está implementado el aviso informativo inicial de Familia.
- Existe una simulación local del aviso previo al vencimiento.
- Los mensajes de tipo Transcripción se almacenan como texto y siguen las mismas reglas de vencimiento que los mensajes de texto.

**Conexión de datos**
- Existe un adaptador local de prueba basado en almacenamiento del navegador.
- Los datos solo están disponibles en el mismo dispositivo y navegador donde se generaron.
- No existe todavía conexión a un servidor, base de datos remota ni sincronización entre integrantes o dispositivos.

**Limitación de validación**
- La publicación de la vista previa de Netlify puede aparecer como exitosa, pero eso por sí solo no significa que la interfaz y todos los flujos hayan sido probados manualmente. En el estado documentado de esta etapa, no se ha completado una validación interactiva integral en navegador. Esa revisión sigue pendiente.

### 3. Qué podemos avanzar ahora, sin elegir todavía un proveedor

Mientras la arquitectura definitiva de datos siga abierta, se puede continuar con tareas que no dependen de esa decisión:

1. **Revisión visual de la interfaz:** revisar la vista previa en distintos tamaños de pantalla, detectar problemas de jerarquía, espaciado, legibilidad, navegación y estados vacíos, y aplicar únicamente las correcciones aprobadas por Vale.
2. **Pruebas funcionales locales:** comprobar los flujos de envío de texto y transcripción, respuestas, primer uso de Familia, inicio de ciclo, vencimiento, renovación y conversaciones de notas.
3. **Casos límite de las reglas temporales:** probar vencimientos exactos, ciclos sin mensajes posteriores, notas sin participación, varias notas con vencimientos independientes y reapertura del módulo después de períodos de inactividad.
4. **Robustez de la lógica:** separar y probar las reglas de negocio frente a la interfaz, evitar que una acción sobre una conversación afecte a otra y comprobar que los datos vencidos no vuelvan a mostrarse.
5. **Estados y experiencia de uso:** revisar estados de carga, ausencia de mensajes, errores locales, confirmaciones y comportamiento de la interfaz cuando no hay conversaciones temporales activas.
6. **Preparación de la capa de datos:** definir y documentar un contrato neutral del adaptador (por ejemplo, operaciones para consultar, crear, responder, actualizar y eliminar mensajes y metadatos), sin implementar todavía una integración externa ni asumir cómo se autenticarán los usuarios.
7. **Mantenimiento de la documentación:** registrar cada decisión aprobada, las limitaciones conocidas y el resultado real de las pruebas.

Estas tareas pueden desarrollarse y revisarse con datos de prueba locales. No requieren decidir ahora dónde vivirán los datos definitivos.

### 4. Pendientes para una etapa posterior

Los siguientes puntos no deben considerarse implementados ni resueltos:

- Elegir, junto a Vale, la arquitectura definitiva de almacenamiento y sincronización. La elección de servidor, base de datos, proveedor o modelo de cuentas permanece abierta.
- Definir autenticación, identidad de cada integrante, pertenencia a una familia, permisos y recuperación de acceso para el Chat.
- Implementar mensajería real entre distintos integrantes y dispositivos, con sincronización y actualización en tiempo real o el mecanismo que se acuerde.
- Garantizar los vencimientos y eliminaciones en el servidor, incluso cuando ningún usuario abra la aplicación.
- Implementar notificaciones fiables a todos los integrantes, incluida la notificación única por integrante y por ciclo 24 horas antes del vencimiento.
- Definir e implementar la eliminación efectiva de los datos remotos y sus metadatos, de acuerdo con las reglas aprobadas y las capacidades del proveedor elegido.
- Incorporar grabación, envío, recepción y almacenamiento de audio original en una versión futura. No forma parte de la versión 1 aprobada.
- La transcripción por voz integrada permanentemente en el compositor de Chat Familia forma parte de la versión 1. Su funcionamiento depende de la compatibilidad de la API de reconocimiento de voz del navegador y del idioma disponible en el dispositivo; no se incorporará un proveedor externo específico.
- Revisar seguridad, privacidad, reglas de acceso, manejo de errores, límites de uso, costos y recuperación ante fallos cuando se evalúe una integración real.
- Ejecutar pruebas integrales con más de un usuario y dispositivo una vez que exista una conexión remota.

### 5. Orden recomendado para continuar

El orden de trabajo recomendado, sujeto a la revisión y aprobación de Vale en cada etapa, es:

1. Revisar visualmente el prototipo actual y recopilar las correcciones de interfaz.
2. Corregir y validar la interfaz del Chat sin intervenir otros módulos.
3. Probar la lógica local de mensajes, respuestas, ciclos y vencimientos, incluidos los casos límite.
4. Completar el contrato de la capa de datos para que sea independiente del proveedor.
5. Revisar con Vale las alternativas de almacenamiento y sincronización, sin elegir una hasta contar con su aprobación expresa.
6. Una vez aprobada la arquitectura, implementar la conexión remota y adaptar el adaptador de datos.
7. Incorporar sincronización multiusuario, notificaciones y vencimientos garantizados en servidor.
8. Realizar pruebas integrales de privacidad, permisos, expiración y funcionamiento entre dispositivos.
9. Evaluar por separado las funciones de audio original para una versión futura.

El orden puede ajustarse según los resultados de las pruebas o las decisiones de Vale. Ningún pendiente de etapas posteriores autoriza a adelantar una elección de proveedor o a modificar el alcance aprobado de la versión 1.
