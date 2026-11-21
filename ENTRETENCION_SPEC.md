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

## 3. Datos propios
- EnOrden guardará preferencias del usuario y favoritos, no el catálogo externo completo.
- Los favoritos conservarán identificadores de origen y metadatos mínimos para poder reconocerlos si cambia el proveedor.
- Cualquier caché será limitada, temporal y separada de los datos personales.

## 4. Filtros
- Tipo: película, serie o indistinto.
- Géneros: selección múltiple; los resultados pueden coincidir con cualquiera de los géneros elegidos.
- Año: rango desde/hasta o cualquier año.
- Los filtros se aplicarán sobre el catálogo consultable completo.

## 5. Ruleta
- Animación real generada por código, adaptable a móvil y escritorio.
- La cantidad de segmentos visibles no limita el tamaño del catálogo.
- Mostrará hasta 12 títulos elegibles elegidos aleatoriamente.
- El resultado debe coincidir exactamente con uno de los segmentos visibles.
- El resultado ganador se mostrará en una ventana superpuesta sobre la ruleta, no como una ficha fija debajo de ella.
- La ventana conservará las acciones de la ficha actual (tráiler, favoritos e información de disponibilidad) para poder revisarlas y ajustarlas posteriormente.
- La ventana tendrá un botón X en la esquina superior derecha para cerrarla y volver a ver la ruleta.
- No se repetirán ganadores dentro de una sesión.
- «Otra oportunidad» elegirá nuevos candidatos entre los títulos elegibles que aún no hayan resultado ganadores.
- «Nueva sesión» reiniciará los ganadores de la sesión, sin borrar favoritos ni preferencias.
- Si hay menos de 12 títulos disponibles, se mostrarán todos.

## 6. Tráileres
- Proveedor inicial: YouTube; será intercambiable independientemente del catálogo.
- Solo se ofrecerán tráileres oficiales del título correcto y en el idioma/variante configurados en EnOrden (por ejemplo, español latino).
- Una búsqueda textual no garantiza oficialidad y no se presentará como verificada.
- Si no se puede verificar canal oficial, título y variante de idioma, se informará que no hay un tráiler oficial disponible.
- La integración futura requerirá un mecanismo fiable, como canales oficiales de estudios/distribuidoras y metadatos lingüísticos.

## 7. Idioma
- Interfaz y metadatos localizables seguirán el idioma global de EnOrden.
- Se conservará la variante regional cuando esté disponible (por ejemplo, español latinoamericano frente a español de España).
- Para tráileres, la variante regional es requisito de coincidencia.

## 8. Seguridad y condiciones
- Las credenciales de proveedores externos no se expondrán en el código cliente.
- Antes de publicar se revisarán términos de uso, atribución, límites de API y licencias, especialmente para uso comercial.
- **PENDIENTE / RECORDATORIO COMERCIAL:** si EnOrden pasa a ser una aplicación comercial, recordar a Vale revisar y confirmar la licencia de la base de datos/proveedor de películas y series (inicialmente TMDB), sus condiciones de uso comercial, atribución, restricciones y posibles costos antes de lanzar o monetizar el servicio.
- Si se incorpora disponibilidad de streaming, deberá indicar la región consultada y la fecha de actualización.

## 9. Pendientes de producción

- **REVISAR / PROPUESTA NO IMPLEMENTADA:** evaluar Cloudflare Workers como intermediario seguro entre EnOrden (alojado en Netlify) y TMDB u otros proveedores. El objetivo sería evitar que las consultas al catálogo consuman créditos de funciones de Netlify y mantener las credenciales fuera del cliente.
- Antes de elegir esta alternativa, revisar los límites y costos vigentes del plan gratuito de Cloudflare Workers, compararlos con el uso esperado, comprobar el estado del plan/créditos de Netlify y probar seguridad, caché, paginación, errores y aislamiento de fallos. La propuesta no es todavía una decisión definitiva.
- Integrar TMDB mediante una capa segura de servidor/proxy y revisar sus condiciones.
- Resolver búsqueda y verificación de tráileres oficiales por idioma/variante.
- Definir si la disponibilidad de streaming en Chile forma parte de la primera versión pública.
- Persistir favoritos/preferencias en la cuenta del usuario cuando exista sistema de cuentas.
- Añadir estados de carga, error, catálogo vacío, límites del proveedor y funcionamiento sin conexión.
