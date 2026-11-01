# TallerPro MVP — Gestor de órdenes de servicio

Aplicación web para que un taller mecánico registre clientes, vehículos y órdenes de servicio, y dé seguimiento a cada reparación desde que el vehículo llega hasta que se entrega.

> Proyecto de portafolio (nivel 1). Objetivo técnico: practicar el ciclo completo de desarrollo (requisitos, diseño, implementación, pruebas, despliegue y mantenimiento) con Django.

## Problema

Muchos talleres llevan sus órdenes en libretas u hojas sueltas: se pierde el historial de cada vehículo, no se sabe en qué estado está cada reparación y no hay forma rápida de consultar qué refacciones se usaron.

## Alcance del MVP

**Incluye**
- Autenticación con dos roles: **administrador** y **mecánico**
- CRUD de clientes, vehículos y órdenes de servicio
- Refacciones/conceptos asociados a cada orden, con cantidad y precio
- Flujo de estados de la orden: `Recibido → En proceso → Listo → Entregado`
- Búsqueda y filtros (por cliente, placa, estado, fecha)
- Vista de orden imprimible (comprobante para el cliente)

**No incluye (mejoras para niveles posteriores)**
- API REST, app móvil o de escritorio
- Facturación electrónica
- Control de inventario de refacciones
- Notificaciones por WhatsApp/correo
- Pagos en línea

## Stack

- Python 3 + Django
- SQLite (desarrollo) 
- Plantillas de Django + Bootstrap
- Pruebas: `pytest-django` o el framework de pruebas de Django
- CI: GitHub Actions
- Despliegue: Docker (destino por definir en la fase de despliegue)

## Roadmap

| Hito | Contenido | Estado |
|---|---|---|
| 0 | Repo, entorno virtual, estructura del proyecto, README | Pendiente |
| 1 | Requisitos y diseño (`docs/`) | Pendiente |
| 2 | Autenticación y roles | Pendiente |
| 3 | CRUD de clientes y vehículos | Pendiente |
| 4 | Órdenes de servicio, estados y conceptos | Pendiente |
| 5 | Búsqueda, filtros y vista imprimible | Pendiente |
| 6 | Pruebas y CI | Pendiente |
| 7 | Despliegue y demo en línea | Pendiente |
| 8 | Mantenimiento: changelog, versión 1.0.0 | Pendiente |

## Documentación

- [Requisitos](docs/requisitos.md)
- [Diseño y modelo de datos](docs/diseno.md)

## Cómo ejecutar (se completará en la implementación)

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

## Decisiones técnicas y aprendizajes

_Se irá llenando durante el desarrollo._