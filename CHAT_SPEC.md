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
- La versión 1 permitirá enviar transcripciones como contenido de texto. El usuario podrá escribirlas o utilizar el dictado disponible en el teclado de su dispositivo.
- La versión 1 no grabará, enviará ni almacenará archivos de audio.
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
