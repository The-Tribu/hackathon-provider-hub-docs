# Hackathon Provider Hub Docs

Documentación de integración para el **Hackathon Provider Hub**, el middleware que centraliza el acceso a múltiples APIs de proveedores de plantas solares (Deye, Huawei, Growatt) para los equipos participantes del hackathon.

## ¿Qué encontrarás aquí?

Esta documentación está pensada para los equipos que consumen el middleware. No necesitas gestionar tokens ni credenciales de proveedores: el middleware se encarga de todo automáticamente.

## Proveedores documentados

### Deye (`/deye`)

Guías, ejemplos de endpoints y pruebas de humo para la API de DeyeCloud.

- [Resumen y autenticación](./deye/README.md)
- [01 · Cuenta](./deye/01-account.md)
- [02 · Estaciones (plantas)](./deye/02-station.md)
- [03 · Dispositivos](./deye/03-device.md)
- [04 · Configuración](./deye/04-config.md)
- [05 · Órdenes / comandos](./deye/05-order.md)
- [Smoke test (`smoke_test.sh`)](./deye/smoke_test.sh)

### Growatt (`/growatt`)

Guías, ejemplos de endpoints y pruebas de humo para la API de Growatt OpenAPI v1.

- [Resumen y autenticación](./growatt/README.md)
- [01 · Plantas (plant)](./growatt/01-plant.md)
- [02 · Dispositivos (device)](./growatt/02-device.md)
- [03 · Inversor específico (MIN/TLX, SPH/MIX)](./growatt/03-inverter.md)
- [Smoke test (`smoke_test.sh`)](./growatt/smoke_test.sh)

## Cómo usar el middleware

Todas las peticiones pasan por el middleware, que valida tu API key de equipo e inyecta las credenciales del proveedor de forma transparente:

```
Cliente → Middleware (valida tu key, inyecta token del proveedor) → API del proveedor
```

### Ejemplo rápido

```bash
curl -H "Authorization: tk_xxx..." \
  "http://<host>:8000/deye/v1.0/station/list"
```

Reemplaza `tk_xxx...` por la API key que se asignó a tu equipo y `<host>` por el host del middleware proporcionado por la organización.

## Reglas importantes

- **Solo se permiten `GET`** en la mayoría de rutas. Las rutas `/deye/*` y `/huawei/*` aceptan `POST` cuando la API del proveedor así lo requiere.
- **API key obligatoria** en el header `Authorization`. Las keys inválidas reciben `401`.
- **Rate limit** por key: 60 peticiones por ventana rodante de 60s. Si lo superas, la key queda bloqueada 180s y recibirás `429` con un header `Retry-After`.
- **Nunca verás errores `401`/`403` del proveedor**: el middleware refresca los tokens automáticamente.

## Soporte

Si encuentras un error en la documentación o en algún endpoint, abre un issue en este repositorio.
