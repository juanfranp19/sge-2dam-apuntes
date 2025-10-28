# UT 1.02 - Evolución de los sistemas de información

---
## 1) ¿Por qué mirar hacia atrás?

Porque el software que verás hoy (ERP, CRM, servicios en la nube) **no nació de la nada**. Cada etapa resolvió un problema concreto y dejó aprendizajes. Cuando entiendes el _“de dónde venimos”_, te resulta más fácil **tomar decisiones de diseño**: qué datos son críticos, cómo se integran módulos, cuándo tiene sentido la nube, etc.

> Nota

> Lo que cambia con rapidez es el **catálogo de productos y versiones** (nombres comerciales, licencias, nubes…). La **secuencia de necesidades** a la que responden los sistemas es bastante estable.

---
## 2) Línea temporal comentada (de lo básico a lo integrado)

### 2.1. Sistemas de Procesamiento de Transacciones (TPS)

**¿Qué son?**

Aplicaciones que registran operaciones repetitivas del días a día: ventas, cobros, asientos contables, movimientos de almacén.

**Problemas que resuelven**

La empresa deja de "contar a mano". Se reduce el error humano y se gana velocidad en tareas rutinarias.

**Ejemplo sencillo**

Un TPV en una tienda: cada ticket queda registrado; luego puedes saber el total del día sin hacer sumas manuales.

**Limitación**

Aislados, generan **islas de información:** el TPV sabe de ventas; el almacén, de stock; contabilidad, de facturas... pero **no se hablan.**

---
### 2.2. Automatización de oficina

**¿Qué es?**

Herramientas de productividad: procesadores de texto, hojas de cálculo, correo, presentaciones.

**Aporte real**

Permiten documentar, calcular, comunicar. En PYMES, Excel se convirtió en "minisistema" para todo: listados de clientes, pedidos, stock...

**Limitación**

Excel **no sustituye** sistemas transaccionales: carece de control de concurrentes, trazabilidad y calidad de dato. Además, fomenta **duplicados.**

> Advertencia

> Si tu "sistema de gestión" son hojas de cálculo desconectadas, no tienes sistema: tienes **riesgo.**

---
### 2.3. De los MRP a los ERP

**MRP (Material Requierements Planning)**

Nacen para planificar **materiales** en producción: ¿qué, cuánto y cuándo comprar según las órdenes de fabricación?

**MRP II (Manufactoring Resource Planning)**

Amplían el foco: además de materiales, consideran **recursos** (máquinas, personas, capacidades).

**ERP (Enterprise Resource Planning)**

El paso lógico: integrar **toda** la operativa en **una única plataforma.** Compras, ventas, inventario, finanzas, proyectos, RRHH, etc., **comparten** una base de datos.

**Idea clave**

> ERP = **dato único** + **procesos conectados** + **trazabilidad.**

**Analogía corta**

Pasamos de *instrumentos sueltos* a una **orquesta** con partitura común. Cada módulo "toca" lo suyo, pero siguen el mismo compás (los datos).

---
### 2.4. CRM y la orientación al cliente

Con la expansión de Internet y el comercio electrónico, el enfoque pasa del producto al **cliente.** Aparecen los **CRM (Customer Relationship Management):** contactos, oportunidades, campañas, atención al cliente y postventa.

**Relación con ERP**

ERP cuida la "operativa interna". CRM cuida la relación con el cliente. Integrados, evitan que ventas prometa lo que operaciones no puede cumplir, y que soporte atienda sin contexto.

---
### 2.5. Arquitectura cliente-servidor y web

Para soportar esa integración, se impone la **arquitectura cliente-servidor:** clientes (aplicaciones o navegadores) que piden servicios a un servidor (donde viven lógica de negocio y base de datos). Con la web, el navegador se vuelve el **cliente universal** y se abren las puertas a:

- Acceso remoto sencillo.
- Actualizaciones centralizadas.
- Integraciones mediante **APIs**.

---
### 2.6. SaaS y la nube

Un salto de modelo: del "instalo y mantengo en mi servidor" al "**Software como Servicio** (SaaS)". Pagas por uso, accedes por Internet, el proveedor se encarga de parches, copias y escalado.

**Ventajas**

Menos inversión inicial, mejoras continuas, facilidad para integrar servicios.

**Desventajas**

Dependencia del proveedor y de la conexión, gestión de **datos y cumplimiento** (privacidad, localización, auditoría).

> Nota

> La adopción de SaaS/Cloud crece, pero cada empresa debe **valorar regulación, presupuesto y control.** No hay una respuesta única.

---
## 3) Hilo conductor: el dato como "héroe silencioso"

Si hay un protagonista constante es el **dato:**

- En **TPS:** registrar correctamente.
- En **ofimática:** presentar y comunicar.
- En **MRP/ERP:** **compartir** y **no duplicar**.
- En **CRM:** contextualizar al cliente para decidir mejor.
- En **Web/Saas:** **disponibilidad y seguridad** a escala.

La pregunta que nos guía siempre es: **¿dónde está el dato “verdadero” y quién lo gobierna?** Esa idea se llama **gobierno del dato** (data governance): reglas y responsables para mantener calidad.

---
## 4) ¿Qué cambió en el trabajo del desarrollador DAM?

1. **Integración por defecto**

Antes: programas aislados.

Ahora: **APIs** y eventos por todos lados. No basta con "guardar y mostrar"; hay que **conectar.**

2. **Modelado de datos con visión empresa**

Comprender entidades maestras (clientes, productos, proveedores) y relaciones (pedidos <-> albaranes <-> facturas).

3. **Seguridad y permisos**

No todos deben ver todo. Roles por área (ventas, compras, finanzas) y registros de actividad.

4. **Observabilidad**

Logs, auditoría y métricas. Si algo falla, hay que **reconstruir el rastro.**

5. **Elección de arquitectura**

Cliente-servidor, web, móvil, on-premise vs. nube. Cada decisión impacta en mantenimiento, costes y velocidad.

---
## 5) Ejemplo: de TPS a ERP con trazabilidad

Imagina que empiezas con un TPS de ventas:

- Guardas pedidos en `pedidos`, líneas en `lineas_pedido`.
- Generas facturas en `facturas`.

Con ERP, añades **trazabilidad total:**

```
-- Rastro de un pedido: de pedido → albarán → factura
 
SELECT p.id AS pedido,
 
       a.id AS albaran,
 
       f.id AS factura,
 
       p.fecha AS fecha_pedido,
 
       a.fecha AS fecha_albaran,
 
       f.fecha AS fecha_factura,
 
       f.importe_total
 
FROM pedidos p
 
LEFT JOIN albaranes a ON a.pedido_id = p.id
 
LEFT JOIN facturas f ON f.albaran_id = a.id
 
WHERE p.id = 12345;
```

La consulta muestra cómo un único flujo **recorre módulos**. Ese es el corazón del ERP: _no repito datos, los **encadeno**_.

---
## 6) Ventajas logradas con cada salto

- **TPS:** fiabilidad y velocidad en operaciones repetitivas.
- **Ofimática:** mejor documentación y comunicación.
- **MRP/ERP:** integración, dato único, trazabilidad, informes globales.
- **CRM:** relación con cliente, seguimiento y personalización.
- **Web/SaaS:** accesibilidad, actualizaciones continuas, escalabilidad.

> Analogía breve

> Es como pasar de **apuntar en cuadernos** (TPS) a **tener fichas** (ofimática), luego **una biblioteca ordenada y compartida** (ERP), y finalmente **la biblioteca conectada con su comunidad** en línea (CRM + SaaS).

---
## 7) Lo que hoy cambia rápido (y conviene vigilar)

- **Modelos de licencia** (perpetua vs. suscripción), **costes** y **limitaciones de uso.**
- **Reglamentos de datos** (protección, localización, auditoría).
- **Panorama de proveedores** (fusiones, cambios de nombre, fin de vida de versiones).

> Nota de verificación

> Si vas a **comparar productos concretos** o **elegir un ERP/CRM SaaS**, valida **precios, condiciones y requisitos** en la fecha de tu decisión. Los conceptos del bloque son estables; los **detalles comerciales** cambian.

---
## 8) Errores comunes y cómo evitarlos

1. **Creer que "más herramientas" = "mejor gestión"

\- *Error:* añadir apps sin estrategia genera **isla de datos.**
\- *Solución:* define **qué sistema es maestro** de cada dato (cliente, producto, stock).

2. **Usar Excel como base de datos central**

\- *Error:* pérdidas de control, duplicados, versiones contradictorias.
\- *Solución:* usa Excel para análisis puntual; los maestros viven en el sistema.

3. **Implantar ERP sin limpiar datos**

\- *Error:* arrastras duplicados y errores a un sistema más rígido.
\- *Solución:* **deduplicación** y normalización antes de migrar.

4. **Ignorar la formación del equipo**

\- *Error:* el sistema existe, pero nadie lo usa bien.
\- *Solución:* plan de **adopción** (formación + soporte + quick wins).

5. **Subestimar las integraciones**

\- *Error:* APIs “a última hora” que no cubren casos reales.
\- *Solución:* define **eventos** y **contratos de API** desde el inicio.

6. **Olvidar la trazabilidad**

\- *Error:* no poder reconstruir un pedido-problema.
\- *Solución:* diseña **IDs consistentes**, estados y logs.

7. **Elejir SaaS sin revisar cumplimiento**

\- *Error:* incumplir políticas internas o normativa.
\- *Solución:* checklist legal y de seguridad (ubicación de datos, backups, SLAs).

---
## 9) Resumen

- La evolución va de **registrar operaciones** (TPS) a **integrar procesos** (ERP) y **cuidar al cliente** (CRM).
- El **dato** es el hilo conductor: único, trazable y disponible.
- Cliente-servidor, web y SaaS son **formas** de desplegar y consumir esos sistemas.
- Como desarrollador DAM, tu reto es **modelar bien**, **integrar** y **asegurar.**
- Lo que cambia rápido: **productos, licencias y requisitos legales** --> ¡verifica antes de decidir!
