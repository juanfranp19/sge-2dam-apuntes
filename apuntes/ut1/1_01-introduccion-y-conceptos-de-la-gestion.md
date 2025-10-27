# UT 1.01 - Introducción y conceptos de la gestión empresarial


## 1) ¿Qué significa "gestionar" una empresa?

Dos ideas clave:

- **Recursos:** todo lo que la empresa usa para funcionar (personas, tiempo, dinero, productos, información).
- **Procesos:** pasos encadenados que transforman algo de entrada ("necesidad de un cliente") en una salida con valor ("venta realizada y registrada").

> En el sector público, el objetivo principal no es el beneficio sino *prestar servicio con  eficiencia.* La palabra "gestión" sigue significando lo mismo: organizar recursos y procesos para cumplir objetivos, pero el objetivo cambia (servicio vs. beneficio).

### Concepto básico (definición sencilla)

- **Gestión empresarial:** conjunto de decisiones y acciones que mantienen una empresa en funcionamiento de forma eficiente para alcanzar sus objetivos.


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


## 3) Tipo de sistemas que ayudan a gestionar (visión histórica simple)

A lo largo del tiempo, la información aplicada a la empresa ha pasado por etapas (la verás ampliada en el **Punto 2** del índice):

- **Sistemas de procesamiento de transacciones (TPS):** registran operaciones básicas (ventas, cobros). Son como "notario digital".
- **Automatización de oficina:** correo, documentos, hojas de cálculo. Ayuda al trabajo diario.
- **ERP (Enterprise Resource Planning):** integra procesos clave de la empresa en **una sola aplicación** (compras, ventas, contabilidad, almacén, ...).
- **CRM (Customer Relationship Management):** se centra en **la relación con el cliente** (contactos, oportunidades, campañas).

> ERP y CRM no compiten: **se complementan**. Piensa en ERP = "órganos internos" (operativa), CRM = "oído y voz" hacia el cliente.


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


## 5) Un caso sencillo (historia guiada)

"Ada" dirige una pequeña empresa de desarrollo. Detecta que un cliente **no tiene** sistema de gestión integral. Antes de programar nada, Ada pide a su equipo:

1. **Revisar conceptos básicos** de gestión.
2. **Explorar el mercado**: ¿qué ERPs/CRMs existen?, ¿libres, de pago?, ¿qué requieren?
3. **Valorar la implantación**: coste, tiempo, curva de aprendizaje.

## 6) Conceptos clave, aclarados sin jerga

- **Proceso de negocio**: secuencia de pasos para lograr un resultado ("de pedido a cobro")
- **Documento comercial**: entidades con valor legal o de registro (pedido, albarán, factura).
- **Integración**: que los sistemas compartan datos y eventos **sin duplicados**.
- **Trazabilidad**: seguir un "rastro" desde el origen al final (pedido --> albarán --> factura --> cobro).
- **DATO ÚNICO**: principio de no duplicación de información maestra (clientes, productos, etc.).

## 7) ¿Qué problemas resuelven ERP y CRM en el día a día?

- **Evitar islas de información**: el Excel de ventas no coincide con el de almacén.
- **Acelerar tareas repetitivas**: facturar 200 pedidos sin "picar" datos.
- **Mejorar la atención al cliente**: ver el historial antes de responder.
- **Controlar el negocio**: paneles con KPIs (ventas por día, rotación de stock, margen...).

> Advertencia

> Un ERP/CRM **no es magia**. Si los procesos están mal definidos, el sistema solo **automatiza el caos**. Primero se ordena el proceso, luego se digitaliza.

## 8) Mini-práctica mental

Toma cualquier comercio que conozcas (una librería, por ejemplo) y describe en voz alta:

1. Entrada: ¿cómo llega un pedido? (web, teléfono, en persona).
2. Registro: ¿dónde se apuntan los datos del pedido?
3. Stock: ¿cómo se comprueba si hay existencias?
4. Documento: ¿se genera albarán/factura automáticamente?
5. Cobro: ¿cuándo y cómo se registra?
6. Seguimiento: ¿qué informe quiero al final del día?

## 9) Buenas prácticas desde el rol DAM

