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
