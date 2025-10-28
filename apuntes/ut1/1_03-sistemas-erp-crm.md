# UT 1.03 - Sistemas ERP-CRM

---
## 1) ¿Qué son y para qué sirven?

#### ERP (Enterprise Resource Planning)

Un **ERP** es un sistema que **integra** los procesos clave de una empresa en una **única plataforma** y comparte un **dato maestro** común (clientes, productos, tarifas, impuestos…). Piensa en módulos como compras, ventas, almacén, contabilidad, proyectos, RRHH. Todos “tocan” la misma base de datos y por eso no hay duplicidades ni re-trabajo.

**Para qué sirve:**

- Evitar **islas de información** (el almacén, ventas y contabilidad ven el mismo pedido).
- Aportar **trazabilidad** (del presupuesto al cobro).
- Estandarizar procesos (formas de trabajo repetibles, auditables).

#### CRM (Customer Relationship Management)

Un **CRM** es el sistema que **organiza la relación con clientes** y potenciales clientes (leads): contactos, oportunidades, llamadas, emails, reuniones, incidencias, campañas.

**Para qué sirve:**

- No perder oportunidades ni contextos de conversación.
- Priorizar a quién atender primero (probabilidad de cierre, valor potencial).
- Mejorar la **experiencia de cliente** (historial completo: antes, durante y después de la venta).

> Nota

> ERP y CRM **no compiten:** se **complementan**. El ERP es la “maquinaria” interna que ejecuta; el CRM es el “radar y la voz” con los clientes. Juntos cierran el círculo: **de la primera llamada al cobro final.**

---
## 2) Arquitectura mental: módulos típicos y cómo se hablan

#### Módulos frecuentes en un ERP

- **Compras:** pedidos a proveedores, recepciones, devoluciones.
- **Almacén/Inventario:** ubicaciones, lotes/series, movimientos, stock disponible/comprometido.
- **Ventas:** ofertas/presupuestos, pedidos, albaranes, facturas.
- **Finanzas/Contabilidad:** asientos, impuestos, cobros/pagos, conciliación bancaria, cierres.
- **Proyectos/Producción:** ordenes de fabricación, rutas, materiales, imputación de tiempos/costes.
- **RRHH:** fichas de personal, ausencias, nóminas (según país/solución).

#### Módulos frecuentes en un CRM

- **Contactos y cuentas** (personas y empresas).
- **Leads y oportunidades** (candidatos a venta y su probabilidad/etapa).
- **Actividades** (llamadas, reuniones, tareas).
- **Casos/soporte** (incidencias, tickets).
- **Campañas** (email, eventos, seguimiento).

#### Integración ERP↔CRM (flujo típico)

1. **Captación** (CRM): entra un lead desde la web.
2. **Cualificación** (CRM): validas interés y presupuesto --> se crea oportunidad.
3. **Oferta** (ERP/CRM): generas un **presupuesto** con precios y stock real; los datos maestros (producto, tarifa, impuestos) vienen del ERP.
4. **Pedido** (ERP): al cerrar la oportunidad, se crea el **pedido** en ERP --> reserva stock, planifica entrega.
5. Entrega y factura (ERP): albarán, factura, impuestos y **asiento contable**.
6. **Postventa** (CRM): caso de soporte, satisfacción, venta cruzada.

> Advertencia

> Si ERP y CRM no comparten **datos maestros** (clientes, productos, impuestos), aparecerán diferencias. La **sincronización** debe ser **bidireccional** o claramente **maestro-esclavo** según el dato.

---
## 3) Tres principios que definen a un buen ERP

1. **Integración**

Todos los módulos “conversan” sobre el mismo dato. Si el stock baja en un albarán, la **disponibilidad** se actualiza al instante para todas las áreas.

2. **Modularidad**

No todas las empresas implantan todo a la vez. Puedes empezar con Ventas + Almacén y añadir Contabilidad más adelante. Eso sí, **las piezas encajan** sin “pegamento casero”.

3. **Adaptabilidad**

Cada empresa es un mundo. Los ERPs permiten **parametrizar** (configurar sin programar) y, si hace falta, **extender** con desarrollos. Ojo: personalizar todo sin control puede complicar actualizaciones.

---
## 4) El "rastro" que evita dolores: trazabilidad extremo a extremo

Un ejemplo mínimo de cómo seguir un pedido:

```
-- Cadena de documentos: Presupuesto → Pedido → Albarán → Factura → Cobro
 
SELECT
 
  pre.id AS presupuesto,
 
  ped.id AS pedido,
 
  alb.id AS albaran,
 
  fac.id AS factura,
 
  cob.id AS cobro,
 
  pre.fecha AS f_presu,
 
  ped.fecha AS f_pedido,
 
  alb.fecha AS f_albaran,
 
  fac.fecha AS f_factura,
 
  cob.fecha AS f_cobro,
 
  fac.total_con_impuestos
 
FROM presupuestos pre
 
LEFT JOIN pedidos ped ON ped.presupuesto_id = pre.id
 
LEFT JOIN albaranes alb ON alb.pedido_id = ped.id
 
LEFT JOIN facturas fac ON fac.albaran_id = alb.id
 
LEFT JOIN cobros cob ON cob.factura_id = fac.id
 
WHERE pre.id = 9876;
```

Este tipo de consulta es oro puro para **auditorías**, **soporte** y **mejoras de proceso**: ves cuellos de botella y tiempos entre etapas.

---
## 5) ¿Qué gana una PYME con ERP-CRM?

- **Velocidad y menos errores**: datos se introducen **una sola vez**.
- **Visibilidad**: paneles (KPI) sobre ventas, stock, plazos, cobros.
- **Mejor servicio**: al teléfono ves el **historial** de cada cliente.
- **Decisiones con contexto**: antes de prometer entrega, compruebas stock y plazos reales.
- **Cumplimiento**: asientos automáticos, impuestos consistentes, mejores cierres.

> Nota

> Los **beneficios** aparecen si ordenas el proceso y **capacitás al equipo**. Un ERP mal implantado puede “automatizar el caos”.


