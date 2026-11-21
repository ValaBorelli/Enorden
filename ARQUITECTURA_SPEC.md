# Arquitectura — especificación general de proveedores, fuentes, almacenamiento y actualizaciones

Estado: decisiones acordadas y pendientes identificados.  
Alcance: arquitectura transversal de EnOrden y especificaciones relacionadas con proveedores, fuentes, créditos, almacenamiento, caché, actualizaciones y preparación comercial.  
Rama de trabajo: `EnOrden-Desarrollo`.

Este documento conserva las especificaciones detalladas de arquitectura acordadas. Las reglas rectoras de toda la aplicación permanecen en `ENORDEN_RULES.md`; las reglas funcionales particulares permanecen en el SPEC del módulo correspondiente.

## 1. Arquitectura general de EnOrden

- Cada módulo tendrá su propia configuración, proveedores, fuentes de datos, caché y reglas de funcionamiento.
- Cambiar un proveedor en un módulo no debe alterar otros módulos, aunque compartan una fuente.
- Los proveedores deben poder reemplazarse sin reconstruir la interfaz ni la lógica del módulo.
- Los errores de un proveedor no deben detener el funcionamiento de otros módulos.
- Las credenciales de API deben mantenerse protegidas y nunca exponerse en el código público del navegador.
- La arquitectura debe permitir utilizar intermediarios o servicios de servidor para conectar proveedores externos.
- La aplicación debe prepararse técnicamente para una futura explotación comercial, sin asumir que eso concede autorización legal para monetizarla.

## 2. Fuentes y créditos

- En Configuración, debajo de Ayuda, existirá un desplegable llamado **Fuentes y créditos**.
- Cada nueva base de datos, API o proveedor externo deberá registrarse allí.
- La sección será centralizada: los módulos no tendrán que mantener sus propias copias de las atribuciones.
- Las atribuciones dependerán del proveedor configurado y de la procedencia real de los datos. Si se reemplaza un proveedor, deberán actualizarse las referencias correspondientes.
- Se distinguirán proveedores actuales y fuentes históricas.
- Se conservarán los créditos de fuentes anteriores mientras existan datos que dependan de ellas o sus condiciones exijan mantenerlos.
- No es necesario atribuir individualmente cada pequeño texto de una ficha si una atribución conjunta representa correctamente las fuentes involucradas.

### 2.1. Categorías y visibilidad

| Categoría | Visibilidad |
|---|---|
| Proveedores | Visible |
| Fuentes originales | Visible |
| Atribuciones | Visible |
| Licencias y condiciones (parte informativa) | Visible |
| Estado de conexión | Interna |
| Países y regiones configurados | Interna |
| Fechas de consulta | Interna |
| Procedencia detallada de cada dato | Interna |
| Historial de cambios | Interna |
| Credenciales API | Nunca visibles |

### 2.2. Atribuciones de Entretención

En la ficha de Entretención se mostrará:

**Disponibilidad informada por JustWatch a través de TMDB.**

La atribución de TMDB incluirá su logotipo oficial y el aviso:

> This product uses the TMDB API but is not endorsed or certified by TMDB.

La atribución de JustWatch identificará su participación en los datos de disponibilidad, sin presentarlo como fuente del catálogo, los pósteres o las sinopsis.

Las atribuciones aparecerán según la procedencia real de los datos, no solamente porque el módulo tenga esos proveedores configurados.

## 3. Almacenamiento mínimo necesario

Regla general:

> EnOrden almacenará únicamente los datos necesarios para el funcionamiento de sus módulos, la experiencia del usuario, la administración de caché, la trazabilidad de las fuentes y la conservación de información que el usuario haya decidido guardar.

Cada módulo definirá los datos concretos que necesita conservar en su propio SPEC.

### 3.1. Criterios de almacenamiento

- Conservar únicamente datos necesarios para el funcionamiento de los módulos, la experiencia del usuario, la administración de caché, la trazabilidad de fuentes y la información que el usuario decidió guardar.
- No conservar respuestas completas de API cuando solo se utiliza una parte.
- No conservar copias duplicadas.
- No conservar historiales de todas las consultas.
- No conservar fechas individuales de cada consulta exitosa como historial.
- No conservar datos sin utilidad funcional.
- No conservar versiones antiguas innecesarias.
- No conservar registros técnicos con información sensible innecesaria.

## 4. Reglas de caché de Entretención

Los períodos acordados son:

| Tipo de información | Duración |
|---|---:|
| Títulos y sinopsis | 1 mes |
| Pósteres e imágenes | 1 año |
| Disponibilidad de streaming | 1 semana |

Además:

- Los períodos serán configurables.
- Cada tipo de información tendrá su propio vencimiento.
- La caché de Entretención será independiente de la de otros módulos.
- Se respetarán las condiciones de almacenamiento de cada proveedor.
- Los datos vencidos deberán actualizarse antes de presentarse como vigentes.
- Si falla la actualización, se conservarán temporalmente los últimos datos válidos.
- No se informará al usuario que esos datos no pudieron verificarse recientemente.
- Una consulta fallida no renovará la referencia de última actualización exitosa.
- No se mostrarán fechas de consulta ni actualización en la ficha de resultados.
- Se conservará internamente la referencia de la última actualización exitosa de cada conjunto de datos, sin crear un historial.
- Los datos se organizarán independientemente por contenido, tipo de información y región cuando corresponda.
- Una consulta exitosa que no encuentre plataformas también renovará la referencia interna de caché de disponibilidad por una semana.

### 4.1. Reintentos

- Primer fallo: programar otro intento para 24 horas después.
- Si vuelve a fallar, programar el siguiente intento otras 24 horas después.
- Repetir hasta obtener una respuesta válida.
- Cuando tenga éxito, actualizar la información y eliminar el reintento pendiente.
- No conservar un historial de fallos, solamente la referencia necesaria para el próximo intento.
- Se desea que los reintentos puedan ejecutarse en segundo plano, aunque la aplicación esté cerrada. Esto dependerá de las capacidades de la infraestructura y del sistema operativo.
- Si no existe ejecución en segundo plano, al reactivarse el servicio se deberá ejecutar el reintento pendiente cuando ya hayan transcurrido las 24 horas.

## 5. Disponibilidad de películas y series en Entretención

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

## 6. Plataformas y enlaces

### 6.1. Nombres y marcas

- Se podrán mostrar nombres como Netflix, Prime Video, HBO Max, Disney+, Apple TV, Mercado Play, TCL, Crunchyroll y otras plataformas.
- Se utilizarán para identificar dónde está disponible el contenido.
- EnOrden deberá presentarse como una aplicación independiente.
- No se insinuará una asociación, patrocinio o recomendación oficial sin autorización.
- No se utilizarán logotipos de streaming en la primera etapa.
- La arquitectura podrá admitir logotipos en el futuro si se confirma su autorización.

### 6.2. Primera etapa de enlaces

Cuando el usuario presione el nombre de una plataforma:

- EnOrden intentará abrir su aplicación y buscar el título seleccionado.
- Si no está instalada, intentará llevar al usuario a la tienda de aplicaciones con la búsqueda de esa plataforma.
- El usuario seleccionará el contenido dentro de la plataforma.

Los datos de cada servicio podrán incluir, cuando existan, enlaces universal/de aplicación, web y tienda. La interfaz podrá priorizar un enlace universal/de aplicación y utilizar el enlace de tienda como alternativa cuando el proveedor o el entorno permitan determinar que la aplicación no está disponible.

Un enlace web normal no garantiza que se abra la aplicación instalada ni permite detectar por sí solo si está instalada. No se prometerá ese comportamiento hasta implementar y probar enlaces universales/deep links y fallback específicos para Android e iOS.

### 6.3. Segunda etapa

- Se podrán incorporar enlaces directos a películas o series específicas, cuando exista una dirección confiable y válida para la región correspondiente.
- Cada plataforma deberá probarse individualmente. No se asumirá que todas permiten los mismos enlaces ni que todos los dispositivos se comportan igual.

## 7. Reemplazo de proveedores

- Se utilizará una única base de datos activa para cada catálogo.
- Cuando se decida reemplazarla, se preparará y probará la nueva conexión antes de eliminar la anterior.
- Se evitará mezclar registros de dos catálogos distintos.
- El cambio completo eliminará del sistema activo la base anterior y su caché.
- Se avisará previamente a los usuarios de que la actualización podría afectar sus favoritos.
- Se intentará recuperar favoritos mediante correspondencias confiables.
- No se inventarán equivalencias cuando los identificadores no coincidan.
- La nueva base deberá funcionar con la misma estructura funcional que la anterior.

### 7.1. Pendiente sobre favoritos

La intención expresada es que, el día en que la actualización se haga para todos los usuarios de la app, idealmente se reemplace por completo.

Queda pendiente revisar, cuando llegue el momento de cambiar proveedores, cuánto tiempo se conservará una copia temporal de los favoritos para intentar recuperarlos durante la migración y cuándo se eliminará definitivamente esa copia.

## 8. Actualizaciones y usuarios de prueba

Estas reglas se aplican a futuras actualizaciones importantes de EnOrden, no solamente a cambios de proveedores.

- Habrá usuarios específicos de prueba.
- Las actualizaciones se probarán primero con ese grupo.
- Los usuarios de prueba recibirán notificaciones manuales indicando qué deben probar.
- Después de validar la actualización, los usuarios generales recibirán un aviso con al menos 24 horas de anticipación.
- La publicación general ocurrirá después de la aprobación.
- El sistema permitirá activar una versión para determinados usuarios sin que los demás la reciban accidentalmente.

### 8.1. Pendiente

Queda pendiente desarrollar el sistema de usuarios y grupos de publicación gradual.

## 9. Preparación comercial y condiciones legales

- EnOrden no será comercial por ahora.
- La arquitectura se preparará técnicamente para una futura versión comercial.
- Las condiciones, licencias, permisos, costos y contratos de cada proveedor deberán revisarse antes de monetizar.
- Tener una API gratuita no significa tener autorización comercial.
- La preparación técnica y la autorización legal serán estados independientes.
- Se revisarán los derechos de imágenes, marcas y enlaces antes de incorporar recursos gráficos o funcionalidades que los requieran.

## 10. Pendientes

1. **Reintentos en segundo plano:** escoger la infraestructura que realizará los reintentos cuando EnOrden no esté abierto. No basta con programar un temporizador dentro de la página.
2. **Usuarios y publicación gradual:** desarrollar el sistema de usuarios y grupos de publicación gradual.
3. **Favoritos al reemplazar el catálogo:** revisar cuánto tiempo se conservará una copia temporal de los favoritos para intentar recuperarlos durante la migración y cuándo se eliminará definitivamente esa copia. La intención expresada es reemplazar por completo el catálogo el día en que la actualización se haga para todos los usuarios.
4. **Autorizaciones comerciales y legales:** antes de monetizar, revisar las condiciones, licencias, permisos, costos y contratos de cada proveedor, además de los derechos de imágenes, marcas y enlaces que correspondan.

## 11. Relación con otros documentos

- Las reglas generales de EnOrden se registran en `ENORDEN_RULES.md`.
- Las reglas funcionales particulares de Entretención se registran en `ENTRETENCION_SPEC.md`.
- Este documento conserva las especificaciones de arquitectura transversal y los detalles compartidos de proveedores, fuentes, créditos, almacenamiento y actualizaciones.
- Los SPEC particulares deben respetar las reglas generales de `ENORDEN_RULES.md`.
