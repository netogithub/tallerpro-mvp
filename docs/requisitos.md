# Requisitos — TallerPro MVP

## Actores

- **Administrador:** gestiona usuarios, catálogos y todas las órdenes.
- **Mecánico:** consulta y actualiza las órdenes que le corresponden.

## Requisitos funcionales

| ID | Requisito | Rol | Prioridad |
|---|---|---|---|
| RF-01 | El sistema permite iniciar y cerrar sesión con usuario y contraseña | Todos | Alta |
| RF-02 | El administrador puede crear, editar y desactivar usuarios y asignarles un rol | Admin | Alta |
| RF-03 | Se pueden crear, consultar, editar y eliminar clientes (nombre, teléfono, correo opcional) | Admin | Alta |
| RF-04 | Se pueden registrar vehículos asociados a un cliente (placa, marca, modelo, año, color) | Admin, Mecánico | Alta |
| RF-05 | Se puede crear una orden de servicio para un vehículo con motivo de ingreso, fecha de ingreso y mecánico asignado | Admin | Alta |
| RF-06 | Se pueden agregar conceptos a una orden (refacción o mano de obra) con descripción, cantidad y precio unitario | Admin, Mecánico | Alta |
| RF-07 | La orden calcula automáticamente el total a partir de sus conceptos | Sistema | Alta |
| RF-08 | La orden sigue el flujo de estados Recibido → En proceso → Listo → Entregado; no se puede saltar ni retroceder, salvo el administrador | Admin, Mecánico | Alta |
| RF-09 | El mecánico solo ve y edita las órdenes asignadas a él | Mecánico | Media |
| RF-10 | Se puede buscar por nombre de cliente, placa o número de orden, y filtrar por estado y rango de fechas | Todos | Media |
| RF-11 | Se puede ver el historial de órdenes de un vehículo | Todos | Media |
| RF-12 | Se puede imprimir un comprobante de la orden | Admin | Media |

## Requisitos no funcionales

| ID | Requisito | Criterio de medición |
|---|---|---|
| RNF-01 | Seguridad: contraseñas con hash, protección CSRF y validación de entradas | Usa los mecanismos de Django; sin secretos en el repositorio |
| RNF-02 | Control de acceso por rol en cada vista | Pruebas que verifican acceso denegado a rutas no permitidas |
| RNF-03 | Usabilidad: interfaz responsiva utilizable en móvil y escritorio | Se visualiza correctamente en 360 px y 1280 px de ancho |
| RNF-04 | Rendimiento: listados paginados | Máximo 20 registros por página; respuesta menor a 1 s con 1 000 órdenes |
| RNF-05 | Mantenibilidad: código organizado por apps y con pruebas automatizadas | Cobertura mínima del 70% en modelos y vistas principales |
| RNF-06 | Portabilidad: ejecución reproducible | Se levanta con Docker o con `requirements.txt` siguiendo el README |
| RNF-07 | Los datos persisten y se pueden respaldar | Documentar el procedimiento de respaldo de la base de datos |

## Historias de usuario y criterios de aceptación (principales)

**HU-01 — Registrar una orden de servicio**
Como administrador quiero registrar la entrada de un vehículo para llevar el control de la reparación.
- Dado un cliente y un vehículo existentes, cuando completo el formulario de orden, entonces se crea con estado "Recibido" y un número único.
- Si falta el motivo de ingreso, el sistema muestra un error y no guarda.

**HU-02 — Avanzar el estado de una orden**
Como mecánico quiero cambiar el estado de mi orden para que el administrador sepa su avance.
- Una orden "Recibido" puede pasar a "En proceso", y esta a "Listo".
- El mecánico no puede marcar "Entregado" ni modificar órdenes de otros mecánicos.

**HU-03 — Calcular el total**
Como administrador quiero ver el total de la orden para cobrar al cliente.
- Al agregar, editar o quitar un concepto, el total se actualiza (suma de cantidad × precio unitario).

**HU-04 — Consultar historial de un vehículo**
Como administrador quiero ver las reparaciones previas de un vehículo para dar mejor diagnóstico.
- Al buscar por placa, se listan sus órdenes ordenadas de la más reciente a la más antigua.

## Supuestos

- Un solo taller (sin multi-sucursal ni multi-empresa).
- Una orden pertenece a un solo vehículo y tiene un solo mecánico asignado.
- Los precios se manejan en una sola moneda (MXN).