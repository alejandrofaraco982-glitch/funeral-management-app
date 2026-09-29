# Funeral Management App

Aplicación móvil y API para administrar los servicios de una funeraria: ventas, pólizas, abonos parciales y cobranza. Diseñada para un negocio real y desplegada en producción.

> **Versión de portafolio.** El código de producción es privado porque contiene información de un cliente. Este repositorio documenta el problema, la arquitectura y las decisiones técnicas. No incluye datos reales, credenciales ni detalles de infraestructura.

## Qué resuelve

Un negocio funerario vende servicios y pólizas y cobra en cuotas. Necesitaba dejar de llevar todo en papel y en hojas de cálculo, saber quién debe cuánto y trabajar aunque la conexión falle.

## Funciones

- **Ventas y pólizas:** servicios, tipos de plan, frecuencia de pago, titular y beneficiarios.
- **Abonos parciales:** el saldo se calcula sumando los abonos; nunca se guarda un total que pueda desincronizarse.
- **Cobranza:** listado de cuentas por cobrar con nombre y teléfono del responsable, no solo el monto.
- **Métodos de pago** de una lista fija y anulación de pagos con registro.
- **Roles y permisos** por tipo de usuario, con auditoría de acciones.
- **Inicio de sesión con usuario y PIN**, con bloqueo tras 5 intentos fallidos y mensajes que no revelan si el usuario existe.
- **Uso sin conexión:** los registros creados offline se sincronizan al recuperar la señal.
- **Buscadores** en listas largas y avisos claros de éxito o error.

## Arquitectura

```
App móvil (Expo, TypeScript)  ──HTTPS──▶  API REST (Express, TypeScript)  ──▶  PostgreSQL
                                                     │
                                          TypeORM + migraciones versionadas
```

| Capa | Tecnología |
|---|---|
| Móvil | Expo (EAS Update), TypeScript |
| API | Node.js, Express, TypeScript |
| Datos | PostgreSQL (TypeORM); sql.js para desarrollo local |
| Autenticación | JWT y bcrypt |
| Pruebas | Vitest (31 pruebas del backend) |
| Despliegue | Servicio en la nube con migraciones aplicadas en el build |

## Decisiones técnicas

- **Dinero en centavos enteros** (`utils/dinero`) para evitar errores de redondeo.
- **Fechas en horario local de Venezuela** para que "hoy" coincida con el negocio.
- **Errores controlados** (`AppError` y un manejador central) para no exponer mensajes internos al cliente.
- **Migraciones con verificación** de sumas antes y después de cada cambio de esquema.
- **Compatibilidad entre motores:** fechas opcionales guardadas como texto ISO para que el mismo modelo funcione en PostgreSQL y en la base local de desarrollo.
- **Identificadores temporales** para registros creados offline, filtrados antes de consultar columnas UUID.

## Estructura del proyecto

```
backend/   API, modelos, servicios, migraciones y pruebas
mobile/    App móvil (pantallas, componentes compartidos, cliente API)
```

## Autor

Alejandro Sánchez Rivero · [LinkedIn](https://www.linkedin.com/in/alejandro-s%C3%A1nchez-90841b232/) · Barquisimeto, Venezuela
