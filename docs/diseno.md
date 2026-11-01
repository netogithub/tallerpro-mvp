# Diseño — TallerPro MVP

## Arquitectura

Aplicación monolítica Django con plantillas del lado del servidor (patrón MTV).

Apps propuestas:

| App | Responsabilidad |
|---|---|
| `accounts` | Usuarios, roles y autenticación |
| `clientes` | Clientes y vehículos |
| `ordenes` | Órdenes de servicio, conceptos y flujo de estados |

## Modelo de datos (ER)

```mermaid
erDiagram
    USUARIO ||--o{ ORDEN : "es asignado a"
    CLIENTE ||--o{ VEHICULO : posee
    VEHICULO ||--o{ ORDEN : recibe
    ORDEN ||--o{ CONCEPTO : incluye

    USUARIO {
        int id PK
        string username
        string password
        string rol "admin | mecanico"
        bool activo
    }
    CLIENTE {
        int id PK
        string nombre
        string telefono
        string correo "opcional"
    }
    VEHICULO {
        int id PK
        int cliente_id FK
        string placa "única"
        string marca
        string modelo
        int anio
        string color
    }
    ORDEN {
        int id PK
        int vehiculo_id FK
        int mecanico_id FK
        string motivo_ingreso
        string estado "recibido | en_proceso | listo | entregado"
        datetime fecha_ingreso
        datetime fecha_entrega "opcional"
    }
    CONCEPTO {
        int id PK
        int orden_id FK
        string tipo "refaccion | mano_de_obra"
        string descripcion
        int cantidad
        decimal precio_unitario
    }
```

El total de la orden no se guarda: se calcula como la suma de `cantidad × precio_unitario` de sus conceptos.

## Flujo de estados de la orden

```mermaid
stateDiagram-v2
    [*] --> Recibido
    Recibido --> EnProceso
    EnProceso --> Listo
    Listo --> Entregado
    Entregado --> [*]
```

## Casos de uso

```mermaid
flowchart LR
    Admin((Administrador))
    Mec((Mecánico))
    Admin --> CU1[Gestionar usuarios]
    Admin --> CU2[Gestionar clientes y vehículos]
    Admin --> CU3[Crear orden de servicio]
    Admin --> CU4[Imprimir comprobante]
    Admin --> CU5[Marcar orden como entregada]
    Mec --> CU6[Ver mis órdenes]
    Mec --> CU7[Agregar conceptos]
    Mec --> CU8[Avanzar estado hasta Listo]
    Admin --> CU9[Buscar y filtrar órdenes]
    Mec --> CU9
```

## Rutas principales (propuesta)

| Ruta | Vista | Acceso |
|---|---|---|
| `/login/`, `/logout/` | Autenticación | Todos |
| `/clientes/` | Lista y búsqueda de clientes | Admin |
| `/clientes/<id>/` | Detalle del cliente y sus vehículos | Admin |
| `/vehiculos/<id>/` | Detalle del vehículo e historial | Admin, Mecánico |
| `/ordenes/` | Lista con filtros | Admin, Mecánico (solo las suyas) |
| `/ordenes/nueva/` | Crear orden | Admin |
| `/ordenes/<id>/` | Detalle, conceptos y cambio de estado | Admin, Mecánico (si es suya) |
| `/ordenes/<id>/imprimir/` | Comprobante imprimible | Admin |

## Estructura de carpetas

```
tallerpro-mvp/
├── README.md
├── docs/
├── manage.py
├── config/            # settings, urls
├── accounts/
├── clientes/
├── ordenes/
├── templates/
├── static/
└── tests/
```