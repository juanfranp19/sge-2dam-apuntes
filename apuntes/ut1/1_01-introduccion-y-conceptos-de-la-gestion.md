# UT 1.01 - Introducción y conceptos de la gestión empresarial

---
## 1) ¿Qué significa "gestionar" una empresa?

Dos ideas clave:

- **Recursos:** todo lo que la empresa usa para funcionar (personas, tiempo, dinero, productos, información).
- **Procesos:** pasos encadenados que transforman algo de entrada ("necesidad de un cliente") en una salida con valor ("venta realizada y registrada").

> En el sector público, el objetivo principal no es el beneficio sino *prestar servicio con  eficiencia.* La palabra "gestión" sigue significando lo mismo: organizar recursos y procesos para cumplir objetivos, pero el objetivo cambia (servicio vs. beneficio).

### Concepto básico (definición sencilla)

- **Gestión empresarial:** conjunto de decisiones y acciones que mantienen una empresa en funcionamiento de forma eficiente para alcanzar sus objetivos.

---
## 2) Del dato a la decisión: información que vale

Una empresa vive de **información:** quién compra, qué stock queda, cuánto cuestan las cosas, qué facturas vencen, etc. El problema no es tener datos, sino **convertirlos en decisiones.**

Flujo típico (simplificado):

1. **Captura de datos** (pedido web, TPV en tienda, email, llamada).
2. **Almacenamiento** (base de datos, hojas de cálculo, papeles).
3. **Procesamiento** (sumar, filtrar, cruzar datos).
4. **Visualización** (informes, paneles).
5. **Decisión** (comprar stock, aplicar descuento, priorizar tareas).

Como desarrolladores, vamos a tocar **los puntos 2 y 4:** dónde guardo, cómo proceso, cómo muestro. Y a veces también **1** (formularios, APIs).

> La información duplicada es veneno: provoca **errores** y decisiones incoherentes. Lo ideal es un **dato único, compartido.**

### Ejemplo en SQL

```
-- ¿Cuánto he vendido este mes por categoría?
 
SELECT c.nombre AS categoria, SUM(l.cantidad * l.precio_unitario) AS total_mes
 
FROM lineas_pedido l
 
JOIN productos p ON p.id = l.producto_id
 
JOIN categorias c ON c.id = p.categoria_id
 
WHERE l.fecha BETWEEN '2025-10-01' AND '2025-10-31'
 
GROUP BY c.nombre
 
ORDER BY total_mes DESC;
 
```


---
## 3) Tipo de sistemas que ayudan a gestionar (visión histórica simple)

A lo largo del tiempo, la información aplicada a la empresa ha pasado por etapas (la verás ampliada en el **Punto 2** del índice):

- **Sistemas de procesamiento de transacciones (TPS):** registran operaciones básicas (ventas, cobros). Son como "notario digital".
- **Automatización de oficina:** correo, documentos, hojas de cálculo. Ayuda al trabajo diario.
- **ERP (Enterprise Resource Planning):** integra procesos clave de la empresa en **una sola aplicación** (compras, ventas, contabilidad, almacén, ...).
- **CRM (Customer Relationship Management):** se centra en **la relación con el cliente** (contactos, oportunidades, campañas).

> ERP y CRM no compiten: **se complementan**. Piensa en ERP = "órganos internos" (operativa), CRM = "oído y voz" hacia el cliente.

---
## 4) ¿Por qué debería importarte a ti, desarrollador de aplicaciones multiplataforma?

Porque gran parte del software profesional **vive dentro de empresas.** Muchos proyectos se parecen a esto:

- "Conecta la tienda online con el stock del almacén".
- "Genera facturas a partir de pedidos y envíalas por email".
- "Monta un panel para ver ventas por vendedor y mes".
- "Integra el formulario de la web con el CRM para que no se pierdan leads".

Para hacer bien ese trabajo necesitas **entender el mapa:** qué áreas hay, cómo se hablan y qué información necesitan.

### Mapa mínimo de áreas

- **Ventas y márketing:** pedidos, presupuestos, clientes potenciales.
- **Compras y logística:** proveedores, recepciones, stock.
- **Finanzas y contabilidad:** facturas, cobros / pagos, cierres.
- **Operaciones/producción** (si aplica): fabricación, planificación.
- **RR.HH.:** personal, nóminas, tiempos.

---
## 5) Un caso sencillo (historia guiada)

"Ada" dirige una pequeña empresa de desarrollo. Detecta que un cliente **no tiene** sistema de gestión integral. Antes de programar nada, Ada pide a su equipo:

1. **Revisar conceptos básicos** de gestión.
2. **Explorar el mercado**: ¿qué ERPs/CRMs existen?, ¿libres, de pago?, ¿qué requieren?
3. **Valorar la implantación**: coste, tiempo, curva de aprendizaje.

---
## 6) Conceptos clave, aclarados sin jerga

- **Proceso de negocio**: secuencia de pasos para lograr un resultado ("de pedido a cobro")
- **Documento comercial**: entidades con valor legal o de registro (pedido, albarán, factura).
- **Integración**: que los sistemas compartan datos y eventos **sin duplicados**.
- **Trazabilidad**: seguir un "rastro" desde el origen al final (pedido --> albarán --> factura --> cobro).
- **DATO ÚNICO**: principio de no duplicación de información maestra (clientes, productos, etc.).

---
## 7) ¿Qué problemas resuelven ERP y CRM en el día a día?

- **Evitar islas de información**: el Excel de ventas no coincide con el de almacén.
- **Acelerar tareas repetitivas**: facturar 200 pedidos sin "picar" datos.
- **Mejorar la atención al cliente**: ver el historial antes de responder.
- **Controlar el negocio**: paneles con KPIs (métricas medibles para evaluar el rendimiento de un sistema ERP) (ventas por día, rotación de stock, margen...).

> Advertencia

> Un ERP/CRM **no es magia**. Si los procesos están mal definidos, el sistema solo **automatiza el caos**. Primero se ordena el proceso, luego se digitaliza.

---
## 8) Mini-práctica mental

Toma cualquier comercio que conozcas (una librería, por ejemplo) y describe en voz alta:

1. Entrada: ¿cómo llega un pedido? (web, teléfono, en persona).
2. Registro: ¿dónde se apuntan los datos del pedido?
3. Stock: ¿cómo se comprueba si hay existencias?
4. Documento: ¿se genera albarán/factura automáticamente?
5. Cobro: ¿cuándo y cómo se registra?
6. Seguimiento: ¿qué informe quiero al final del día?

---
## 9) Buenas prácticas desde el rol DAM

- **Normaliza datos maestros** (clientes, productos) y evita duplicados desde la UI.
- **Valida en frontend y backend** (no confíes solo en el cliente).
- **Registra eventos** (creación/edición/cambios de estado) con usuario y marca temporal.
- **Piensa en integraciones** (APIs) desde el primer día: qué expongo, qué consumo.
- **Diseña para informes**: campos calculables, fechas claras, estados bien definidos.

Ejemplo para **no duplicar clientes** por email:

```
async function createCustomer(newCustomer) {
 
  const existing = await api.get(`/customers?email=${encodeURIComponent(newCustomer.email)}`);
 
  if (existing.length > 0) {
 
    throw new Error("Correo ya existente. Revisa si es el mismo cliente.");
 
  }
 
  return api.post("/customers", newCustomer);
 
}
```

---
## 10) Errores comunes y cómo evitarlos

1. **Confundir "datos" con "información"**

\- *Error:* guardar por guardar (cajas llenas de cosas sin etiquetar).
\- *Solución:* define **para qué** se captura cada dato y **quién lo usará.**

2. **Duplicar registros maestros (clientes, productos)**

\- *Error:* “Cliente Acme”, “ACME S.L.”, “Acme SL” = tres clientes falsos.  
\- *Solución:* claves únicas (NIF, email), búsquedas inteligentes y merge controlado.

3. **No documentar el proceso real**

\- *Error:* digitalizar un proceso imaginario que nadie sigue.
\- *Solución:* entrevista rápida a usuarios, dibuja el flujo actual y valida.

4. **Subestimar la calidad de datos**

\- *Error:* arrancar el sistema con datos "sucios" (repetidos, mal escritos).
\- *Solución:* limpieza previa (deduplicación y normalización) y reglas de verificación.

5. **Pensar que el software arregla la cultura**

\- *Error:* instalar un ERP sin formar ni comunicar.
\- *Solución:* plan de formación y *quick wins* visibles (que la gente note la mejora).

6. **Ignorar seguridad y permisos**

\- *Error:* todos ven todo, o nadie puede hacer nada.
\- *Solución:* roles claros (ventas, compras, contabilidad) y revisiones periódicas.

7. **Falta de trazabilidad**

\- *Error:* no se puede reconstruir qué pasó con un pedido.
\- *Solución:* IDs consistentes, estados definidos y logs de cambios.

---
## 11) Lo que sí puede cambiar y conviene verificar

- **Herramientas y proveedores** (nombres, versiones, modelos de licencia) cambian con frecuencia.

Si en este bloque mencionamos productos a modo de ejemplo en clases futuras, **verifica versiones y condiciones** antes de tomar decisiones técnicas o de compra.

- **Normativa fiscal/contable** puede actualizarse según país y año. Aunque aquí solo introduciremos conceptos, cuando pases a facturación real, **consulta la normativa vigente.**

> Nota

> Para este **Punto 1** nos centramos en conceptos relativamente estables (qué es gestionar, por qué centralizar datos, rol de ERP/CRM). La volatilidad viene más por **herramientas concretas** que veremos en puntos posteriores.

---
## 12) Resumen

- Gestionar = coordinar recursos y procesos para objetivos claros.
- El dato por sí solo no vale: hay que convertirlo en decisiones.
- ERP integra procesos internos; CRM cuida la relación con clientes.
- Dato único y trazabilidad sin cimientos de cualquier sistema serio.
- El software potencia buenos procesos... o acelera el caos si están mal definidos.
