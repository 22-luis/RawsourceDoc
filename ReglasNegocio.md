## Reglas de Negocio del Sistema

* **Inventario del Provider:** Un producto debe estar en el inventario del *provider* para poder ser entregado.
* **Entrega de Orden:** Al entregar una orden, los productos y cantidades se **suman al inventario del *buyer***.
* **Eliminación de Producto:**
    * Solo el ***provider*** o el ***admin*** pueden eliminar un producto (si está habilitado).
* **Aprobación y Entrega de Órdenes:**
    * Solo el ***provider*** puede aprobar y entregar órdenes.
* **Cancelación de Órdenes:**
    * Solo el ***buyer*** o el ***provider*** pueden cancelar una orden.
* **Eliminación de Órdenes:**
    * Solo el ***buyer*** puede eliminar una orden, y solo si está en estado **PENDING**.
* **Modificación de Items de Orden:**
    * Solo el ***buyer*** puede modificar los items de una orden, y solo si está en estado **PENDING**.