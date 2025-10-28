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

> Los **beneficios aparecen** si ordenas el proceso y **capacitás al equipo**. Un ERP mal implantado puede “automatizar el caos”.

---
## 6) ¿Y qué hay de las desventajas o límites?

- **Coste y esfuerzo inicial:** licencias (o suscripciones), implantación, formación, limpieza de datos.
- **Cambio cultural:** hay que **seguir el proceso** (no atajos).
- **Riesgo de sobrediseño:** querer implantar "todo" a la vez. Mejor **por fases**.
- **Dependencia tecnológica:** en SaaS, dependes del proveedor y su **SLA**; en on-premise, dependes de tu **equipo y hardware**.

---
## 7) Interoperabilidad práctica: APIs y sincronizaciones

En la vida real, el ERP no está solo: tienda online, marketplace, app móvil, banco, transportista...

**Buenas prácticas:**

- **Máster de datos:** decide dónde "vive" la verdad (ej.: productos y tarifas en ERP; leads en CRM).
- **Eventos:** "cuando se emite una factura, dispara un webhook".
- **Contratos claros:** APIs versionadas, con ejemplos de payload y errores.
- **Sincronización incremental:** por marcas de tiempo/ID para no saturar.

```
// Al enviar un formulario de "pide tu demo"
 
const lead = await crm.createLead({
 
  nombre, email, origen: "web", consentimientoRGPD: true
 
});
 
await crm.createActivity({
 
  leadId: lead.id,
 
  tipo: "tarea",
 
  descripcion: "Llamar en 24h",
 
  vencimiento: addDays(new Date(), 1)
 
});
```

---
## 8) Caso guiado

- **Situación:** la empresa cliente no tiene sistema integral.
- **Paso 1:** María limpia **maestros** (clientes/productos) en una hoja, deduplica y normaliza.
- **Paso 2:** Instalan un ERP sencillo con módulos **Ventas + Almacén + Facturación** y un **CRM** con **leads + oportunidades.**
- **Paso 3:** Integran **ofertas** del ERP con **oportunidades** del CRM (mismo numeración visible, enlaces cruzados).
- **Paso 4:** Definen 3 KPI:

	- Tiempo **oportunidad → pedido.**
	- **Rotación** de stock.
	- **Días de cobro** (DSO).

- **Resultado:** en 2-3 semanas ven menores errores de facturación y mejor priorización comercial. El resto de módulos (contabilidad/BI) lo dejan para la segunda fase.

---
## 9) Errores comunes y cómo evitarlos

1. **Duplicar datos maestros entre ERP y CRM**  

\- *Error:* cada sistema “crea a su manera” clientes/productos.  
\- *Solución:* define **quién es maestro** y sincroniza **IDs** y **campos clave**.

2. **No cerrar el circuito oferta→pedido**  

\- *Error:* presupuestos “flotantes” que no se convierten ni se miden.  
\- *Solución:* transición **explícita** (estado) y métricas de conversión.

3. **Medir solo ventas, no cobros**  

\- *Error:* celebrar ingresos que nunca se cobraron.  
\- *Solución:* KPI de **cobro real** y alertas de vencimientos.

4. **Personalizar sin control**  

\- *Error:* tocar el núcleo para casos raros → actualizaciones dolorosas.  
\- *Solución:* primero **parametriza**; si programas, que sea **extensión** desacoplada.

5. **Ignorar permisos y privacidad**  

\- *Error:* todo el mundo ve datos sensibles (precios, RRHH).  
\- *Solución:* **roles** por área y mínimos necesarios (_least privilege_).

6. **No formar al equipo**  

\- *Error:* la herramienta existe, pero cada uno trabaja “como siempre”.  
\- *Solución:* guías cortas, vídeos de 3–5 min y soporte inicial.

7. **Integraciones “rápidas” sin contrato**  

\- *Error:* parches frágiles que fallan en silencio.  
\- *Solución:* **APIs documentadas**, pruebas y monitorización.

---
## 10) Lo que puede cambiar y conviene verificar

- **Catálogo de productos, versiones y licencias** de ERPs/CRMs (on-premise vs. SaaS) cambia con frecuencia. Antes de decidir, verifica precios, límites de usuarios, almacenamiento y SLAs vigentes.

- **Normativa** (contable, fiscal, protección de datos) puede actualizarse por país y año. Si vas a emitir facturas reales o tratar datos personales, **confirma requisitos actuales** (por ejemplo, retenciones, tipos de IVA, RGPD).

- **Integraciones externas** (APIs de bancos, marketplaces, transportistas) también cambian endpoints, autenticación o cuotas; revisa documentación **reciente**.

---
## 11) Resumen

- **ERP** = integra operaciones internas con **dato único** y **trazabilidad**.
- **CRM** = gestiona la **relación con el cliente** (antes/durante/después de la venta).
- Juntos cierran el ciclo: **lead → oportunidad → oferta → pedido → entrega → factura → cobro → soporte**.
- El éxito depende de **datos limpios**, **procesos claros**, **formación** y **permisos**.
- Personaliza con cabeza y documenta **integraciones** desde el día 1.
