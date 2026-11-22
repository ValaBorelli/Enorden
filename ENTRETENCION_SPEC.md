# Entretención — reglas funcionales

Estado: reglas acordadas; primera etapa de ruleta animada.
Rama: EnOrden-Desarrollo. Este documento aplica exclusivamente a Entretención.

## 1. Independencia
- Entretención administra sus propias funciones y estado.
- Puede leer preferencias globales de EnOrden (por ejemplo, idioma) mediante interfaces definidas.
- No debe modificar ni afectar el funcionamiento interno de otros módulos.

## 2. Catálogo y proveedor
- El catálogo de películas y series será externo; no se almacenará completo dentro de EnOrden.
- Primer proveedor: The Movie Database (TMDB).
- La integración estará detrás de una interfaz intercambiable; cambiar de proveedor no exigirá reconstruir la interfaz ni el motor de la ruleta.
- Se consultará el catálogo completo que permita el proveedor según los filtros, no solamente tendencias o títulos populares.
- La integración deberá respetar paginación, límites y condiciones del proveedor.
- Los datos recibidos se normalizarán al formato interno de Entretención.

## 3. Datos propios, almacenamiento y caché

### 3.1. Datos que se conservarán

EnOrden conservará únicamente información que tenga una función concreta en Entretención:

- Identificadores de películas y series.
- Títulos y datos descriptivos necesarios.
- Rutas o referencias de imágenes.
- Disponibilidad por plataforma.
- País o región de disponibilidad.
- Procedencia de los datos.
- Referencias de caché, conservando únicamente la última referencia necesaria.
- Favoritos del usuario. Los favoritos conservarán identificadores de origen y metadatos mínimos para poder reconocerlos si cambia el proveedor.
- Relaciones entre identificadores de proveedores, cuando exista una correspondencia confiable.

### 3.2. Datos que no se conservarán

- Respuestas completas de API cuando solo se utiliza una parte.
- Copias duplicadas.
- Historial de todas las consultas.
- Fechas individuales de cada consulta exitosa como historial.
- Datos sin utilidad funcional.
- Versiones antiguas que ya no sean necesarias.
- Registros técnicos con información sensible innecesaria.

Se conservará internamente la referencia de la última actualización exitosa de cada conjunto de datos, sin crear un historial de consultas exitosas.

### 3.3. Política de caché

- Cualquier caché será limitada, temporal y separada de los datos personales.
- La caché conservará únicamente los datos externos necesarios para mejorar el rendimiento y evitar consultas repetidas; no se convertirá en una copia permanente del catálogo completo.
- La política de actualización será independiente según el tipo de dato:
  - **Títulos y sinopsis:** 1 mes.
  - **Pósteres e imágenes:** 1 año.
  - **Disponibilidad de streaming:** 1 semana.
- Los períodos serán configurables y cada tipo de información tendrá su propio vencimiento.
- La caché de Entretención será independiente de la de otros módulos.
- Se respetarán las condiciones de almacenamiento de cada proveedor.
- Los datos vencidos deberán actualizarse antes de presentarse como vigentes.
- Si falla la actualización, se conservarán temporalmente los últimos datos válidos.
- No se informará al usuario que esos datos no pudieron verificarse recientemente.
- Una consulta fallida no renovará la referencia de última actualización exitosa.
- No se mostrarán fechas de consulta ni actualización en la ficha de resultados.
- Los datos se organizarán independientemente por contenido, tipo de información y región cuando corresponda.
- Una consulta exitosa que no encuentre plataformas también renovará la referencia interna de caché de disponibilidad por una semana.

### 3.4. Reintentos

- Primer fallo: programar otro intento para 24 horas después.
- Si vuelve a fallar, programar el siguiente intento otras 24 horas después.
- Repetir hasta obtener una respuesta válida.
- Cuando tenga éxito, actualizar la información y eliminar el reintento pendiente.
- No conservar un historial de fallos, solamente la referencia necesaria para conocer el momento del fallo y el próximo intento.
- Los intentos automáticos en segundo plano son deseables para mantener la información actualizada aunque la aplicación esté cerrada. La programación real dependerá de las capacidades del servicio de ejecución elegido; no se debe prometer una ejecución puntual en dispositivos móviles si la plataforma no puede garantizarla.
- Si no existe ejecución en segundo plano, al reactivarse el servicio se deberá ejecutar el reintento pendiente cuando ya hayan transcurrido las 24 horas.
- La política de caché debe implementarse en una capa desacoplada del proveedor, para que cambiar de proveedor o de infraestructura no obligue a rehacer la lógica de Entretención.
## 4. Filtros
- Tipo: película, serie o indistinto.
- Géneros: selección múltiple; los resultados pueden coincidir con cualquiera de los géneros elegidos.
- Año: rango desde/hasta o cualquier año.
- Los filtros se aplicarán sobre el catálogo consultable completo.

## 5. Ruleta
- Animación real generada por código, adaptable a móvil y escritorio.
- La ruleta utiliza una estética de casino clásico/europeo: rueda circular con profundidad, aro y detalles dorados, segmentos en negro, rojo oscuro, marfil y dorado, y una bolita blanca claramente visible.
- La paleta visual de la ruleta es fija e independiente de la personalización de colores del tema global de EnOrden.
- La rueda debe permanecer centrada y completamente dentro del ancho disponible, con márgenes de seguridad en pantallas pequeñas; el conjunto conserva proporción circular.
- El texto central «ENORDEN» debe permanecer contenido en el centro sin desbordarse.
- Los nombres de los segmentos deben tener ancho limitado según la cantidad de títulos y truncarse con puntos suspensivos cuando sea necesario para evitar solapamientos.
- La rueda y la bolita giran en sentidos opuestos con arranque suave, velocidad inicial moderada y desaceleración progresiva; al final, la bolita realiza pequeños rebotes antes de asentarse en el segmento ganador.
- La animación representa el resultado que ya selecciona la lógica de Entretención; no introduce un segundo sorteo ni modifica el catálogo, los filtros o las reglas de selección.
- La cantidad de segmentos visibles no limita el tamaño del catálogo.
- Mostrará hasta 12 títulos elegibles elegidos aleatoriamente.
- El resultado debe coincidir exactamente con uno de los segmentos visibles.
- El resultado ganador se mostrará en una ventana superpuesta sobre la ruleta, no como una ficha fija debajo de ella.
- La ventana conservará las acciones de la ficha actual (reseñas, favoritos e información de disponibilidad) para poder revisarlas y ajustarlas posteriormente.
- La ventana tendrá un botón X en la esquina superior derecha para cerrarla y volver a ver la ruleta.
- La configuración de la ruleta se abrirá en una ventana modal; sus controles (tipo, géneros y rango de años) deben permanecer dentro de ella.
- Abrir o cancelar la configuración no debe borrar los filtros aplicados, el catálogo cargado, los favoritos, el resultado ni el progreso de la sesión. Los cambios solo se aplican al pulsar «Listo»; «Cancelar», la X o tocar fuera de la ventana solo la cierran.
- No se repetirán ganadores dentro de una sesión.
- «Otra oportunidad» elegirá nuevos candidatos entre los títulos elegibles que aún no hayan resultado ganadores.
- «Nueva sesión» reiniciará los ganadores de la sesión, sin borrar favoritos ni preferencias.
- Si hay menos de 12 títulos disponibles, se mostrarán todos.

## 6. Reseñas

- La ficha de películas y series tendrá un botón llamado exactamente **«Ver reseñas»**.
- En Android, el botón intentará abrir directamente la búsqueda interna de TikTok con el título, año cuando exista y términos relacionados con reseña, crítica y opinión.
- En otros entornos se abrirá la búsqueda web de TikTok.
- La búsqueda se construye dinámicamente; EnOrden no incorpora, reproduce, almacena ni selecciona videos de TikTok.
- La selección y reproducción del contenido queda a cargo del usuario.
- El acceso a TikTok no debe insinuar patrocinio, asociación o respaldo de TikTok hacia EnOrden.
- No se utilizarán logotipos ni recursos de marca de TikTok.
- La implementación actual utiliza un enlace profundo de Android probado en el dispositivo de Vale. Su comportamiento deberá volver a comprobarse en otros dispositivos y versiones de TikTok.
- **PENDIENTE FUTURO:** evaluar alternativas solo si la búsqueda deja de funcionar; no incorporar API ni extracción automatizada de contenido sin una revisión específica de permisos y condiciones.

## 7. Ficha del resultado

- El resultado ganador aparece en una ventana modal superpuesta a la ruleta. La X superior derecha la cierra y devuelve la vista a la ruleta.
- El encabezado de la ventana dice exactamente «Tu resultado».
- El contenido principal se organiza en dos columnas: póster a la izquierda, con aproximadamente un tercio del ancho útil, y texto a la derecha. El póster conserva sin cambios su fondo, tratamiento, proporción y ajuste de imagen actuales.
- La columna de texto muestra, en este orden: título alineado con la parte superior del póster; metadatos pequeños y gris claro (tipo, año y temporadas o duración cuando exista); descripción; géneros; botón único «Ver reseñas»; sección «Dónde verla» con los servicios disponibles como chips.
- La sección «Dónde verla» permanecerá en la ficha. Cuando una consulta exitosa no encuentre plataformas, no se mostrarán opciones de plataformas ni un mensaje que indique que no se encontraron. Si la consulta falla, el contenido dependerá de las reglas de la sección 9.
- El botón de favoritos se ubica debajo de «Ver trailer» y utiliza exactamente el mismo estilo visual que los demás botones de acciones de la ficha (fondo, borde, esquinas, tipografía y espaciado).
- El ícono de corazón aparece a la izquierda del texto y es ligeramente más pequeño que en la versión anterior. Cuando el título no está guardado muestra ♡, con interior transparente y contorno negro; cuando está guardado muestra ♥ relleno de negro.
- El texto cambia según el estado: «Añadir a favoritos» cuando no está guardado y «En favoritos» cuando sí lo está.
- Al pulsarlo agrega o quita el título de favoritos mediante la función y persistencia existentes.

### 7.1. Fichas abiertas desde Favoritos

- Cada película o serie de «Mis favoritos» será seleccionable.
- Al seleccionar un favorito se abrirá la misma ventana modal de resultados que utiliza la ruleta, conservando su presentación, metadatos, reseñas, disponibilidad, favoritos y créditos según procedencia.
- La ficha abierta desde Favoritos tendrá además el botón **«Programar»**. Esta acción no aparecerá en la ficha que se abre desde la ruleta.
- Al pulsar «Programar» se abrirá una ventana independiente con el texto «¿Para cuándo quieres programar tu Film?».
- La ventana incluirá un botón de calendario para elegir fecha, un botón «Hora» que utilizará el mismo selector de hora en formato rueda del Calendario y una casilla «Todo el día».
- Si se marca «Todo el día», se ocultará la selección de hora y no se configurará una alerta horaria.
- «Guardar» creará un evento de Calendario llamado «Ver [título]», de tipo interno `film`, mostrado en el Calendario como **«Evento Film»**.
- El evento se creará con prioridad alta, color caqui (`#C3B091`), calendario personal y sin repetición.
- Si se indica una hora, se guardará una alerta de 15 minutos antes y una notificación en el centro de notificaciones de EnOrden.
- Si se marca «Todo el día», no se guardará hora ni alerta; se generará una notificación de EnOrden indicando que ese Film está programado para ese día.
- El evento podrá editarse desde el Calendario en todos sus campos habituales, excepto el tipo, que permanecerá bloqueado como «Evento Film».
- «Cancelar» y la X de la esquina superior derecha solicitarán confirmación antes de abandonar la programación. La X se ubicará parcialmente fuera del borde superior derecho de la ventana.
- Tocar fuera de la ventana no la cerrará ni tendrá efecto.
- La confirmación de cancelación debe conservar la fecha, hora y opción «Todo el día» seleccionadas si el usuario decide seguir programando.
- Al pulsar «Guardar», la ventana de programación se cerrará y se mostrará una confirmación titulada «Tu Film se programó con éxito!», con la fecha elegida y la hora elegida o «Todo el día».
- Si se eligió una hora, la confirmación indicará: «Te llegará una notificación 15 minutos antes de la hora seleccionada». Este mensaje no aparecerá para eventos de todo el día.
- La confirmación incluirá, en texto más pequeño y gris claro: «Puedes modificar tu evento Film en el calendario», y un botón «Entendido».
- La programación no modificará el favorito ni el catálogo de Entretención.

- La ficha debe adaptarse a móvil manteniendo el orden visual, la legibilidad y el póster a la izquierda siempre que el ancho lo permita.
- En la parte inferior de la ventana se conservarán los créditos de los proveedores de datos, separados del contenido de disponibilidad.
- La atribución del catálogo identificará a TMDB e incluirá su logotipo oficial y el aviso: “This product uses the TMDB API but is not endorsed or certified by TMDB”.
- La atribución de disponibilidad indicará: «Disponibilidad informada por JustWatch a través de TMDB».
- El logotipo de TMDB debe ser uno de los recursos oficiales aprobados, conservar sus colores y proporciones sin modificaciones y ocupar un lugar menos prominente que la identidad de EnOrden. Debe enlazar a https://www.themoviedb.org.
- Los créditos no deben presentar a JustWatch como fuente de títulos, pósteres o sinopsis, ni sugerir que TMDB o JustWatch patrocinan o respaldan EnOrden.
- Las atribuciones aparecerán según la procedencia real de los datos, no solamente porque el módulo tenga esos proveedores configurados.
## 8. Idioma
- La preferencia de idioma de tráileres se derivará del idioma global de EnOrden.
- Para español, la variante inicial será español latino (es-419), seguida de español de España (es-ES) y luego inglés.
- Para inglés y portugués, se priorizará el idioma configurado.
- La lista de idiomas alternativos se ampliará cuando EnOrden incorpore nuevas preferencias de idioma.
- Las etiquetas de búsqueda indicarán claramente el idioma o variante que se está consultando.
- La búsqueda textual de YouTube no garantiza que los resultados estén en el idioma solicitado; el usuario debe comprobar el video antes de reproducirlo.

## 9. Disponibilidad, plataformas, enlaces, seguridad y condiciones

### 9.1. Disponibilidad de películas y series

- Se utilizará TMDB inicialmente para consultar la disponibilidad mediante su API.
- JustWatch será reconocido como fuente original de la información de disponibilidad.
- Se consultará la región correspondiente, inicialmente Chile.
- La sección «Dónde verla» permanecerá en la ficha del resultado.
- Las plataformas se mostrarán mediante sus nombres escritos, no mediante logotipos.
- La información se mostrará agrupada por modalidad cuando corresponda: suscripción, alquiler, compra, gratis y gratis con publicidad.
- Si existe una sola opción, se mostrará directamente sin crear una sección innecesaria.
- No se inventarán plataformas ni disponibilidad.
- Si una consulta falla y existen datos anteriores, se conservarán esos datos sin mostrar una alerta ni una fecha.
- Si falla y no existen datos anteriores, se informará que no fue posible obtener la disponibilidad.
- Si una consulta exitosa no encuentra plataformas, no se mostrarán opciones de plataformas ni se ofrecerá una consulta alternativa desde EnOrden.
- Una respuesta exitosa sin plataformas se considera una consulta exitosa y renueva la referencia interna de caché por una semana.

### 9.2. Plataformas y marcas

- Se podrán mostrar nombres como Netflix, Prime Video, HBO Max, Disney+, Apple TV, Mercado Play, TCL, Crunchyroll y otras plataformas.
- Se utilizarán para identificar dónde está disponible el contenido.
- EnOrden deberá presentarse como una aplicación independiente.
- No se insinuará una asociación, patrocinio o recomendación oficial sin autorización.
- No se utilizarán logotipos de streaming en la primera etapa.
- La arquitectura podrá admitir logotipos en el futuro si se confirma su autorización.

### 9.3. Enlaces a plataformas

**Primera etapa**

Cuando el usuario presione el nombre de una plataforma:

- EnOrden intentará abrir su aplicación y buscar el título seleccionado.
- Si no está instalada, intentará llevar al usuario a la tienda de aplicaciones con la búsqueda de esa plataforma.
- El usuario seleccionará el contenido dentro de la plataforma.

**Segunda etapa**

- Se podrán incorporar enlaces directos a películas o series específicas, cuando exista una dirección confiable y válida para la región correspondiente.
- Cada plataforma deberá probarse individualmente. No se asumirá que todas permiten los mismos enlaces ni que todos los dispositivos se comportan igual.

Los datos de cada servicio deberán llegar desde un proveedor de disponibilidad intercambiable e incluir, cuando existan, enlaces universal/de aplicación, web y tienda. La interfaz podrá priorizar un enlace universal/de aplicación y utilizar el enlace de tienda como alternativa cuando el proveedor o el entorno permitan determinar que la aplicación no está disponible. Un enlace web normal no garantiza que se abra la aplicación instalada ni permite detectar por sí solo si está instalada. No se prometerá ese comportamiento hasta implementar y probar enlaces universales/deep links y fallback específicos para Android e iOS.

Para cada plataforma, el orden de enlaces será: `universalUrl`, `appUrl`, `webUrl`. `storeUrl` solo podrá usarse como alternativa cuando el proveedor o el entorno indique explícitamente que la aplicación no está disponible (por ejemplo, `appAvailable=false` o `appInstalled=false`).

### 9.4. Estado de consulta y presentación

- La capa de disponibilidad normalizará el estado de cada consulta usando `streamingAvailabilityStatus` o `streamingAvailability.status`, con los valores `success` y `error`.
- Una respuesta exitosa debe marcarse explícitamente como `success`, incluso cuando la lista de servicios esté vacía.
- Una respuesta exitosa con lista vacía no mostrará opciones de plataformas ni el mensaje «Lo sentimos, no se encuentran plataformas de Streaming disponibles».
- Una respuesta fallida debe marcarse como `error`. Si hay datos de una consulta exitosa anterior, se entregarán mediante `lastSuccessfulStreamingProviders` o `streamingAvailability.lastSuccessfulProviders` y se mostrarán esos servicios sin alterar su estado de última consulta exitosa.
- Una respuesta fallida sin datos anteriores mostrará exactamente: «No se pudo obtener la disponibilidad en este momento».
- La ausencia de estado explícito no se interpretará como una consulta exitosa sin resultados.
- La región consultada, las fechas de consulta o actualización y los demás metadatos técnicos del proceso de búsqueda se conservarán únicamente para uso interno. No se mostrarán en la ficha ni al usuario.

### 9.5. Seguridad y condiciones

- Las credenciales de proveedores externos no se expondrán en el código cliente.
- Antes de publicar se revisarán términos de uso, atribución, límites de API y licencias, especialmente para uso comercial.
- **PENDIENTE / RECORDATORIO COMERCIAL:** si EnOrden pasa a ser una aplicación comercial, recordar a Vale revisar y confirmar la licencia de la base de datos/proveedor de películas y series (inicialmente TMDB), sus condiciones de uso comercial, atribución, restricciones y posibles costos antes de lanzar o monetizar.
- Se respetarán las condiciones de almacenamiento de cada proveedor.
- Las atribuciones de TMDB y JustWatch aparecerán según la procedencia real de los datos, no solamente porque los proveedores estén configurados.

## 10. Pendientes de producción
- **REVISAR / PROPUESTA NO IMPLEMENTADA:** evaluar Cloudflare Workers como intermediario seguro entre EnOrden (alojado en Netlify) y TMDB u otros proveedores. El objetivo sería evitar que las consultas al catálogo consuman créditos de funciones de Netlify y mantener las credenciales fuera del cliente.
- Antes de elegir esta alternativa, revisar los límites y costos vigentes del plan gratuito de Cloudflare Workers, compararlos con el uso esperado, comprobar el estado del plan/créditos de Netlify y probar seguridad, caché, paginación, errores y aislamiento de fallos. La propuesta no es todavía una decisión definitiva.
- Integrar TMDB mediante una capa segura de servidor/proxy y revisar sus condiciones.
- Evaluar e implementar, si se autoriza, una API de YouTube u otro proveedor para localizar tráileres específicos y obtener metadatos fiables.
- **PENDIENTE DE VALIDACIÓN:** comprobar que la consulta de disponibilidad de streaming en Chile mediante el proveedor elegido funciona de forma estable y entrega resultados confiables. Esta función no es todavía un requisito obligatorio para la primera versión pública.
- Una vez realizadas las pruebas y revisado el trabajo restante del módulo, decidir si la disponibilidad de streaming se incluye en el lanzamiento inicial o se reserva para una actualización posterior.
- Persistir favoritos/preferencias en la cuenta del usuario cuando exista sistema de cuentas.
- Añadir estados de carga, error, catálogo vacío, límites del proveedor y funcionamiento sin conexión.
- **PENDIENTE:** incorporar infraestructura de notificaciones en segundo plano si se requiere entregar alertas cuando EnOrden esté cerrado. La implementación actual procesa los avisos de Film mientras la aplicación está abierta o cuando vuelve a primer plano.


### Pendientes derivados de las reglas de arquitectura

- **PENDIENTE:** desarrollar el sistema de usuarios y grupos de publicación gradual para activar actualizaciones para determinados usuarios sin que los demás las reciban accidentalmente.
- **PENDIENTE:** escoger la infraestructura que realizará los reintentos cuando EnOrden no esté abierto. No basta con programar un temporizador dentro de la página.
- **PENDIENTE:** revisar, cuando llegue el momento de cambiar proveedores, cuánto tiempo se conservará una copia temporal de los favoritos para intentar recuperarlos durante la migración y cuándo se eliminará definitivamente esa copia. La intención expresada es que, el día en que la actualización se haga para todos los usuarios de la app, idealmente se reemplace por completo.
- **PENDIENTE:** antes de monetizar, revisar las condiciones, licencias, permisos, costos y contratos de cada proveedor, además de los derechos de imágenes, marcas y enlaces que correspondan.
