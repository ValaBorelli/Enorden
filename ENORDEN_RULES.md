# EnOrden — reglas generales del proyecto

**Documento rector:** estas reglas se aplican a todo EnOrden y prevalecen sobre las especificaciones particulares de cada módulo.

**Rama de trabajo autorizada:** `EnOrden-Desarrollo`  
**Rama protegida:** `main`

## 1. Flujo de trabajo y control de versiones

- Todo desarrollo, prueba y corrección se realizará primero en la rama `EnOrden-Desarrollo`, que funciona como espacio de pruebas.
- No se realizarán cambios directamente en `main`, ni se fusionarán ramas hacia `main`, ni se publicarán cambios definitivos sin una solicitud explícita de Vale.
- Una autorización para modificar `main` debe ser expresa y referirse a esa acción. No se interpretará el silencio, una aprobación de otra tarea ni una solicitud de desarrollo como autorización para tocar `main`.
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
