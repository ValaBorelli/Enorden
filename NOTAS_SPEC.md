# Notas — especificación del módulo

## 1. Propósito
Notas es una pizarra visual de corcho donde los integrantes del hogar pueden colocar papeles con texto. La organización espacial es libre y cada nota conserva su posición.

## 2. Superficie y presentación
- La textura de corcho ocupa el fondo completo del área del módulo.
- Las notas se representan como papeles individuales con chinche superior.
- Cada nota permite elegir el color del papel al crearla.
- El texto publicado utiliza exactamente la tipografía oficial **EnOrden Pizarra**, reutilizada desde `assets/fonts/Caveat-Variable.ttf` y declarada en el proyecto para la pizarra de Compartidos.
- El texto admite escritura con teclado y saltos de línea. No admite emojis, dibujos ni trazos manuales.
- El tamaño, la posición y la ligera orientación visual se conservan al mover o volver a mostrar una nota.

## 3. Creación y privacidad
- Al crear una nota, el usuario escribe el contenido, selecciona el color y define su alcance:
  - **Personal:** solo la persona creadora puede verla y administrarla.
  - **Familiar:** todos los integrantes de la familia pueden verla y administrarla.
- La nota registra a su creador. Solo esa persona puede realizar la corrección única.
- El modelo de datos debe conservar identificador, texto original, color, alcance, creador, fecha de creación, posición, orientación, estado de corrección y estado de papelera.

## 4. Movimiento y papelera
- Para mover una nota, se mantiene presionado el chinche superior y se arrastra el papel.
- El movimiento debe funcionar con mouse y pantallas táctiles.
- Al soltar una nota sobre la papelera, se marca como eliminada y deja de mostrarse en la pizarra.
- La papelera es un botón flotante circular en la esquina inferior derecha, por encima de la navegación inferior. Su ilustración representa una papelera de escritorio metálica negra, construida en SVG y con cuerpo de malla.
- Cada nota enviada a la papelera aparece como una bola de papel arrugado dentro de la papelera. La ilustración muestra hasta 10 papeles simultáneamente; al restaurar o vaciar notas, la cantidad visible se actualiza. La entrada de cada papel tiene una breve animación.
- Al abrirla, se presenta una lista de las notas eliminadas visibles para el usuario.
- Cada nota tiene una acción individual «Devolver» que la restaura a la pizarra.
- «Vaciar papelera» solicita confirmación y elimina definitivamente las notas que contiene.
- Las notas familiares y su papelera son colaborativas; las personales y su papelera son privadas.

## 5. Corrección única
- El texto de una nota publicada no se puede editar ni reemplazar silenciosamente.
- La persona creadora puede corregir la nota una sola vez.
- La corrección puede abarcar una palabra, un fragmento, una oración o todo el texto.
- La pizarra muestra visualmente el texto original modificado tachando los fragmentos reemplazados y mostrando los fragmentos nuevos en su lugar.
- La corrección queda registrada de forma permanente y no puede repetirse.
- Los demás integrantes pueden mover o enviar al papelero una nota familiar, pero no corregirla.
- No se admiten emojis ni dibujos en el texto original ni en la corrección.

## 6. Almacenamiento y limitaciones de prototipo
- La implementación inicial utiliza almacenamiento local del navegador.
- Las notas personales y familiares se guardan localmente en el dispositivo. En esta etapa, «Familiar» representa el alcance lógico del dato dentro del prototipo, pero **no sincroniza cambios entre cuentas ni dispositivos**.
- La colaboración familiar real, la identificación autenticada de cada creador y la autorización en servidor requieren backend compartido. No presentar esta versión local como sincronización multiusuario real.
- Al migrar a backend, las reglas de privacidad, autoría, corrección única, papelera y permisos familiares deben validarse del lado del servidor.

## 7. Alcance de implementación
- Notas mantiene su lógica, estilos y almacenamiento dentro de su módulo.
- No modificar otros módulos, navegación, cabecera ni componentes globales como parte de la implementación de Notas.
