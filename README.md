

# Fastura

**Fastura** es una plataforma web multi-instancia para la gestión empresarial y la facturación electrónica. Permite organizar ventas, compras, inventario, clientes, proveedores, puntos de venta, restaurantes, reportes y configuración visual desde un mismo panel.

Fastura permite operar con múltiples instancias independientes, con datos, usuarios, configuraciones y personalización separados por cliente o tenant.

## 🚀 Características principales

- 🧾 **Facturación electrónica:** emisión y gestión de comprobantes electrónicos, series, correlativos, XML, PDF, CDR y estados ante SUNAT.
- 🛒 **Ventas y comercio:** documentos de venta, notas de venta, clientes, vendedores, órdenes, cotizaciones y oportunidades de venta.
- 📦 **Compras:** registro de compras, proveedores, percepciones, pagos, órdenes de compra y seguimiento de obligaciones.
- 🏪 **Punto de venta (POS):** interfaz rápida para ventas, cobros, caja, impresión de tickets y consulta de operaciones.
- 🍽️ **Restaurante:** mesas, pedidos, mozos, consumo por mesa, cobros y operación de salones.
- 📦 **Inventario y almacén:** productos, categorías, unidades, almacenes, Kardex, lotes, transferencias y control de existencias.
- 📈 **Dashboard y reportes:** indicadores de ventas, compras, utilidades, clientes, productos, inventario y comportamiento del negocio.
- 👥 **Usuarios y permisos:** roles, niveles de acceso, establecimientos, series y permisos por módulo.
- 🏢 **Multi-instancia:** soporte para tenants independientes mediante `hyn/multi-tenant`, con bases de datos y configuraciones separadas.
- 🎨 **Personalización visual:** temas CSS, modo claro/oscuro, interfaz responsive y personalización por instancia.
- 🔌 **API e integraciones:** API REST, Postman, servicios SOAP/XML, notificaciones, reportes y procesamiento de archivos.
- 📄 **Documentos y exportación:** generación de PDF, códigos QR, códigos de barras, Excel, archivos XML y documentos de venta.
- 🧾 **Configuración operativa:** datos de empresa, establecimientos, series, moneda, impuestos, unidades, condiciones de pago y parámetros generales.
- 🔐 **Seguridad y control:** autenticación, autorización, bloqueo de usuarios, auditoría de operaciones y protección de acciones sensibles.

## 🛠️ Tecnologías utilizadas

### Backend

- **PHP** 7.1+
- **Laravel** 5.7
- **Eloquent ORM** y Query Builder
- **Hyn Multi-Tenant** para arquitectura multi-instancia
- **Guzzle** y **Goutte** para integraciones y consumo de servicios
- **Symfony Process** para tareas y procesos del servidor

### Frontend

- **Vue.js** 2
- **Element UI** para componentes y formularios
- **Bootstrap** 4
- **jQuery** y **Axios**
- **Chart.js** para gráficas del dashboard
- **Moment.js** para fechas y filtros
- **Vuex** para el estado de la aplicación
- **CKEditor**, **Vue Draggable**, **Dropzone** y otros componentes de edición y carga

### Datos y documentos

- **MySQL/MariaDB**
- **Migraciones y semillas de Laravel**
- **MPDF**, **Dompdf** y **FPDF** para documentos PDF
- **Maatwebsite Excel** para reportes y exportaciones
- **XML** y librerías de firma para documentos electrónicos
- **Socket.IO** para eventos y notificaciones en tiempo real

### Herramientas de desarrollo

- **Laravel Mix** y **Webpack**
- **Sass**
- **Composer** y **npm**
- **Docker** y **Apache/Nginx** para despliegues


## Términos y condiciones del uso de este repositorio

1.- Este repositorio es de código abierto pero de acceso privado, se permite la distribución y/o modificaciones si se hace referencio a la casa matriz del desarrollo de este es software es [https://facturaloperu.com](https://facturaloperu.com)

2.- Esta sección de términos y condiciones no puede ser removida al compartir o distribuir el repositorio de alguna forma, de hacerlo, [https://facturaloperu.com](https://facturaloperu.com) se reserva el derecho de remover el acceso y limitar el uso a quien lo distribuya de esa forma o a quien se atribuya el desarrollo del mismo.

3.- Si desea distribuir el código fuente como propio, debe tener al menos un 30% de modificaciones en todo el código, y previamente debe validarse dicho % por [https://facturaloperu.com](https://facturaloperu.com)

4.- El uso del software a nivel funcional es marca blanca, sin embargo a nivel de distribución del código fuente, debe contener esta sección de términos y condiciones.

5.- [https://facturaloperu.com](https://facturaloperu.com) no se hace responsable por los daños o perjuicios del uso del código de este software cuando no ha sido distribuido directamente por [https://facturaloperu.com](https://facturaloperu.com)
