# Soluciones EPP Store · V21.2 Cloud

Entrega Cloud lista para operación comercial. Conserva la identidad visual existente y usa Supabase como fuente central de datos para todos los dispositivos.

## Incluye

- `index.html`: administración Cloud.
- `catalogo.html`: catálogo público sin precios, carrito y solicitud de cotización.
- `manifest.webmanifest`: base PWA.
- `sw.js`: caché básica para instalación/uso resiliente.
- `vercel.json`: configuración para publicación estática en Vercel.
- `assets/sidebar_reference.png`: referencia visual de la identidad existente.

## Ya conectado

Project URL:
`https://xioqgscjyetsuzrfrrwf.supabase.co`

La aplicación utiliza exclusivamente la **publishable key** en el navegador.
No incluye ninguna secret key, service_role ni contraseña de PostgreSQL.

## Flujo que ya soporta

1. Login administrativo con Supabase Auth.
2. Dashboard central.
3. Perfiles `admin`, `commercial`, `inventory` y `viewer`, con navegación limitada según el rol y permisos reforzados por la base de datos.
4. Crear/editar productos y gestionar el inventario mediante movimientos transaccionales.
5. Crear/editar clientes, revisar solicitudes, convertirlas en cotizaciones y registrar pagos.
6. Catálogo público conectado a `get_public_catalog`, con las 127 referencias e imágenes Cloud ya cargadas.
7. Cada referencia abre una ficha técnica comercial con descripción, características, aplicaciones, recomendaciones y advertencias provenientes de la base. La ficha no sustituye la documentación del fabricante.
8. Las vistas adicionales recuperadas de V20 se encuentran en `../v21_product_migration/images/gallery/`. Después de cargarlas al bucket `product-public`, ejecutar `../v21_product_migration/sql/12_gallery_views.sql` para habilitarlas en las fichas.
7. Carrito público con cantidad, talla/especificación y observaciones por referencia.
8. Envío del carrito como solicitud mediante `submit_public_quote_request`; el visitante no ve precios.
9. Solicitud → cotización mediante `convert_request_to_quote`.
10. Cotización → venta + cuenta de cobro + salida de inventario mediante `convert_quote_to_sale`.
11. Registro de pagos mediante `register_payment`.
12. Realtime para productos, clientes, solicitudes, cotizaciones, inventario, cartera y pagos.

## Perfiles operativos

| Perfil | Puede operar |
| --- | --- |
| `admin` | Toda la plataforma, configuración y asignación de perfiles. |
| `commercial` | Clientes, solicitudes, cotizaciones, cuentas de cobro y pagos. |
| `inventory` | Productos e inventario. |
| `viewer` | Consulta de productos e inventario. |

Para incorporar una persona, primero se crea su cuenta en Supabase Authentication y luego un administrador le asigna el perfil desde **Accesos**. No se deben compartir cuentas entre personas.

## Publicación recomendada

La carpeta completa puede publicarse como sitio estático.

En Vercel:
1. Crear un proyecto nuevo.
2. Subir/importar esta carpeta o su contenido.
3. Framework Preset: `Other`.
4. No hay build command.
5. Output Directory: dejar vacío.
6. Deploy.

Después, en Supabase:
Authentication -> URL Configuration
- Site URL: URL generada por Vercel.
- Redirect URLs: agregar la misma URL y, más adelante, el dominio personalizado.

## Estado de datos

Los 127 productos, sus categorías e imágenes públicas ya están en la base Cloud. No se migraron clientes, usuarios operativos, inventario ni documentos históricos de V20: se acordó iniciar esos registros nuevamente en V21.
