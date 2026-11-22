# Configuración — Especificación del módulo

## 1. Alcance

Este documento registra las reglas específicas de Configuración, accesible desde el engranaje de la cabecera global de EnOrden. Las reglas generales de ventanas de `ENORDEN_RULES.md` se mantienen vigentes.

## 2. Confirmación al guardar imagen de perfil

- Cuando el usuario guarde correctamente una nueva imagen de perfil, fotografía o avatar desde **Configuración → Mi Perfil → Imagen de Perfil**, EnOrden mostrará una ventana de confirmación.
- La ventana aparecerá únicamente después de que el guardado haya finalizado correctamente.
- El título será **«Imagen de perfil actualizada»** y el mensaje indicará que la imagen de perfil se guardó correctamente.
- La ventana utilizará la categoría base **confirmation** del sistema general de ventanas y ofrecerá el botón **«Entendido»**.
- Si el guardado falla, se mostrará el aviso de error correspondiente y no se mostrará una confirmación de éxito.

## 3. Confirmación al guardar datos de contacto

- Cuando se guarde correctamente un dato de contacto editable de **Configuración → Mi Perfil → Contacto**, EnOrden mostrará una ventana de confirmación.
- Esta regla incluye, entre otros, el correo electrónico secundario y el número de teléfono o WhatsApp.
- En los datos que requieren verificación, la confirmación se mostrará solamente después de que la verificación termine correctamente y el dato quede guardado.
- Cada confirmación identificará el dato guardado. Por ejemplo:
  - **«Correo alternativo guardado»**: indica que el correo secundario se guardó y verificó correctamente.
  - **«Número de WhatsApp guardado»**: indica que el número de teléfono se guardó y verificó correctamente.
- Las ventanas utilizarán la categoría base **confirmation** del sistema general y ofrecerán el botón **«Entendido»**.
- No se mostrará una confirmación de éxito si la validación falla, la verificación no coincide o el dato no llega a guardarse.
- Los datos de contacto no editables, como el correo de acceso, no generan una confirmación porque no se pueden modificar desde esta sección.

## 4. Límites de esta regla

- Esta regla no se extiende automáticamente al cambio del nombre de usuario ni a otras opciones de Configuración que no hayan sido indicadas.
- No modifica el proceso de verificación, los datos almacenados ni los permisos de edición; únicamente establece la confirmación posterior al guardado exitoso.
- Las confirmaciones respetarán las reglas generales de diseño y comportamiento de ventanas definidas en `ENORDEN_RULES.md`.
