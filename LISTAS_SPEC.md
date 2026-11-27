# EnOrden — módulo Listas

## 1. Propósito
Listas permite mantener una lista de productos organizada por secciones para apoyar las compras del hogar.

## 2. Alcance actual
Esta especificación registra el comportamiento existente y la primera corrección funcional del módulo. No define todavía funciones futuras que no hayan sido acordadas.

### Secciones
Los productos se organizan en estas secciones, en este orden:
1. Despensa
2. Congelados
3. Frutas
4. Verduras
5. Limpieza
6. Cocina
7. Baño
8. Personales
9. Extras
10. Listo

El formulario actual de creación permite elegir entre las primeras nueve secciones. «Listo» se mantiene como sección de visualización y no aparece como destino en el selector de creación.

### Productos
Cada producto utiliza actualmente estos campos:
- `id`: identificador numérico.
- `name`: nombre del producto.
- `section`: sección a la que pertenece.
- `bought`: estado de compra (`true` o `false`).

### Comportamientos
- El botón global «+» abre el formulario «Agregar producto» cuando el módulo Listas está activo.
- El formulario permite escribir el nombre y seleccionar la sección de destino.
- «Agregar producto» valida que exista un nombre y una sección seleccionada.
- Al confirmar, el producto se incorpora a la sección elegida, con `bought: false`.
- Después de agregarlo, los cambios se guardan y la vista de Listas se actualiza.
- Cada producto puede marcarse como comprado y eliminarse.

## 3. Almacenamiento actual
- Los productos se mantienen en el arreglo `products`.
- La persistencia del prototipo utiliza `localStorage` con la clave `enorden_products`.
- El guardado se realiza mediante la función general `save()`.
- Este almacenamiento es local al dispositivo/navegador y no representa sincronización entre usuarios o dispositivos.

## 4. Dependencias e interfaces
- Listas utiliza el botón global «+» y el sistema existente de modal.
- La navegación y el contenedor principal de la aplicación son compartidos; este módulo no debe modificarlos para implementar funciones propias.
- El módulo utiliza las funciones existentes `save()`, `closeModal()`, `render()` y `showWindowNotice()`.

## 5. Límites y pendientes
- No hay todavía un sistema de listas múltiples ni colaboración multiusuario definido en esta especificación.
- No se deben asumir reglas de cantidades, unidades, precios, historial, proveedores, categorías personalizadas o sincronización hasta que Vale las defina.
- Cualquier ampliación del comportamiento o del modelo de datos debe documentarse aquí y respetar `ENORDEN_RULES.md`.
