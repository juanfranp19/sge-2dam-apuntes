# UT 1.04 - Implantación del ERP en la empresa

---
## 1) ¿Por qué y para qué?

Un ERP no se instala "porque sí". Se implanta para **resolver problemas concretos:** duplicaciones de datos, falta de visibilidad de stock, retrasos en facturación, errores contables... Si no hay **objetivos claros,** el proyecto se difumina.

> Nota

> En una frase: *"La implantación empieza en un documento, no en el servidor."* Ese documento es el **alcance**: qué módulos, qué procesos, qué resultados esperamos y en qué plazos.

**Ejemplos de objetivos SMART** (concretos y medibles):

- Reducir en **30%** los errores de facturación en 3 meses.
- Tener **inventario valorizado** al cierre de cada día, con una tolerancia de ±2%.
- Acordar el ciclo **pedido → cobro** de 45 a **30 días** en dos trimestres.

---
## 2) Roles del proyecto (quién hace qué)

- **Sponsor** (dirección): desbloquea decisiones y presupuesto; es “la cara” del proyecto.
- **Jefe/a de proyecto** (cliente): coordina, prioriza y cuida el alcance; evita “pedidos de última hora”.
- **Consultoría/Implantador**: guía, parametriza, migra datos, entrena al equipo.
- **Key users** (usuarios clave por área): ventas, compras, almacén, finanzas; validan procesos y pruebas.
- **Equipo técnico** (si on-premise): sistemas/redes/bases de datos; si es **SaaS**, este rol se reduce.

> Analogía breve

> Piensa en una **obra**: el sponsor es el propietario, el implantador el arquitecto/constructor, y los key users son quienes vivirán allí y deciden dónde van los enchufes.

---
## 3) Fases de una implantación (mapa de ruta práctico)

### 3.1. Descubrimiento y análisis (2-4 semanas típicas en PYME)

- **Levantamiento de procesos**: “as-is” (cómo trabajáis hoy) y “to-be” (cómo debería ser).  
- **Mapa de datos maestros**: clientes, productos, tarifas, impuestos, proveedores.  
- **Riesgos y dependencias**: integraciones con tienda online, TPV, contabilidad previa, bancos.

**Entregable:** documento de alcance + lista priorizada de requisitos (MVP y mejoras posteriores).

### 3.2. Parametrización (configurar, no programar)

- Impuestos, series de documentos, roles y permisos, reglas de numeración, unidades de medida, plantillas de documentos.
- Estados del flujo: **presupuesto → pedido → albarán → factura → cobro**.

**Pro tip:** deja **comentarios** dentro del propio ERP (si lo permite) o un **runbook** en tu wiki con cada decisión de configuración y su motivo.

### 3.3. Migración de datos (limpieza + importación)

- **Limpieza** previa: deduplicar, normalizar, completar campos obligatorios.
- **Mapeo de campos**: del sistema antiguo (o Excel) al ERP nuevo.
- **Importación** por lotes: primero maestros (clientes, productos), luego saldos de stock, por último documentos abiertos (pedidos pendientes, facturas a cobrar).

*Script de control de unicidad (conceptual):*

```
-- Detección de posibles duplicados por NIF y por email
 
SELECT nif, COUNT(*) AS repeticiones
 
FROM clientes_import
 
GROUP BY nif
 
HAVING COUNT(*) > 1
 
  
 
UNION ALL
 
  
 
SELECT email, COUNT(*) AS repeticiones
 
FROM clientes_import
 
WHERE email IS NOT NULL AND email <> ''
 
GROUP BY email
 
HAVING COUNT(*) > 1;
```

### 3.4. Integraciones (si las hay)

- **ERP ↔ tienda online/marketplace**: catálogo, stock y precios **solo desde el maestro** (ERP).
- **ERP ↔ banco**: conciliación; ficheros o APIs.
- **ERP ↔ transportistas**: etiquetas, tracking.
- **ERP ↔ CRM**: oportunidades ↔ ofertas/pedidos.

Define **contratos de API** y prueba con datos de ejemplo y casos límite (devoluciones, impuestos especiales).

### 3.5. Pruebas (no te las saltes)

- **Funcionales**: cada área valida sus escenarios habituales.
- **Integradas**: simula un día real: alta de cliente → pedido → albarán → factura → cobro → asiento.
- **Rendimiento básico**: ¿los listados críticos responden en segundos, no minutos?
- **Corte (“cut-over”) planificado**: fecha en la que dejas de usar el sistema viejo y arrancas el nuevo.

> Advertencia

> La prueba “de papel” no vale. Que **ventas** facture un pedido real, **almacén** haga una recepción, **finanzas** cierre un asiento. Sin eso, el arranque es a ciegas.

### 3.6. Formación y manuales típicos

- Guías de **2–3 páginas** por rol con capturas y “cómo hago X”.
- Mini-vídeos de **3–5 minutos** para tareas frecuentes.
- Calendario de “**aula de guardia**” la primera semana para dudas.

### 3.7. Arranque y soporte (la hora de la verdad)

- Arranque un **lunes por la mañana** con el implantador “en sitio” o conectado.
- Canales de soporte claros (ticket, chat) y tiempos de respuesta pactados.
- Reunión de **retrospectiva** a la semana y al mes: ¿qué duele?, ¿qué mejorar?

---
## 4) Plan por fases

Intentar implantar **todo** a la vez suele salir mal. Propón **fases**:

1. **Fase 1 (MVP)**: Ventas + Almacén + Facturación + Informes básicos.
2. **Fase 2**: Compras avanzadas + Reaprovisionamiento + Conciliación bancaria.
3. **Fase 3**: Contabilidad analítica + Proyectos/Producción + BI.
4. **Fase 4**: Automatizaciones y optimizaciones (workflows, alertas).

Cada fase debe cerrar con **KPI visibles** (ver sección 6). Lo que no aporta valor inmediato, **fuera del MVP**.

---
## 5) On-premise vs. SaaS (decisión informada)

- **SaaS (nube):**

	- Menor inversión inicial, actualizaciones automáticas, acceso remoto sencillo.
	- Dependencia del proveedor, límites de personalización, debes revisar **SLAs** y **ubicación de datos.**

- **On-premise (en tus servidores):**

	- Control total, personalización profunda, sin cuotas por usuario (según licencia).
	- Necesitas equipo técnico, responsable de backups/seguridad/actualizaciones, inversión inicial mayor.

> Nota

> Esta decisión debe considerar **coste total de propiedad (TCO)** en 3-5 años, no solo el primer mes.

---
## 6) Métricas que importan (para saber si vamos bien)

- **DSO (Days Sales Outstanding)**: días desde factura hasta cobro.
- **Tasa de error de facturación**: nº de facturas corregidas / total.
- **Stock inmovilizado**: % del inventario sin rotación > 90 días.
- **Exactitud de inventario**: diferencias entre físico y sistema.
- **Lead time pedido→entrega**: días promedio.
- **Adopción**: % de operaciones realizadas en el ERP (vs. atajos fuera).

*Ejemplo de consulta de rotación (visión conceptual):*

```
-- Rotación por producto (ventas / stock medio en el trimestre)
 
WITH ventas_trimestre AS (
 
  SELECT producto_id, SUM(cantidad) AS uds_vendidas
 
  FROM lineas_factura
 
  WHERE fecha BETWEEN '2025-07-01' AND '2025-09-30'
 
  GROUP BY producto_id
 
),
 
stock_medio AS (
 
  SELECT producto_id, AVG(cantidad) AS stock_promedio
 
  FROM snapshot_stock_diario
 
  WHERE fecha BETWEEN '2025-07-01' AND '2025-09-30'
 
  GROUP BY producto_id
 
)
 
SELECT p.nombre, v.uds_vendidas, s.stock_promedio,
 
       CASE WHEN s.stock_promedio = 0 THEN NULL
 
            ELSE ROUND(v.uds_vendidas / s.stock_promedio, 2) END AS rotacion
 
FROM productos p
 
LEFT JOIN ventas_trimestre v ON v.producto_id = p.id
 
LEFT JOIN stock_medio s ON s.producto_id = p.id
 
ORDER BY rotacion DESC NULLS LAST;
```

---
## 7) Gestión del cambio (el factor humano)

- **Comunica el “para qué”**: qué gana cada área (no sólo la empresa).
- **Pilotos** con usuarios reales antes del arranque.
- **Quick wins** visibles en la primera semana (informes o automatismos que ahorren tiempo).
- **Feedback** estructurado (formulario corto) y mejora continua.

---
## 8) Riesgos frecuentes y cómo mitigarlos

- **Alcance elástico** (entra todo lo que aparece):

  → _Mitigación_: backlog priorizado; lo que no es crítico, a la siguiente fase.

- **Datos sucios** que bloquean:

  → _Mitigación_: semana de limpieza previa + reglas de validación en el alta.

- **Dependencias externas** (APIs que fallan):

  → _Mitigación_: colas/reintentos, alertas, _feature flags_ para desactivar integraciones temporalmente.

- **Sobrecarga del equipo clave**:

  → _Mitigación_: reservar horas reales en agenda; apoyo temporal en tareas operativas.

- **Permisos mal definidos** (todos ven todo):

  → _Mitigación_: matriz de roles por área y principio de **mínimo privilegio**.

---
## 9) Checklist de arranque (práctico y corto)

- Copia de seguridad/verificación de **datos importados**.
-  **Usuarios y roles** creados y probados.
-  **Series de numeración** y **plantillas** de documentos activas.
-  **Listado de pedidos abiertos** cargado y validado.
-  **Cuadres de stock** hechos (artículos críticos revisados físicamente).
-  **Integraciones** con éxito en un flujo de extremo a extremo.
-  **Plan B** si algo falla (volver temporalmente al sistema anterior o plan de contingencia manual).
-  **Soporte en caliente** asignado para la primera semana.

---
## 10) Lo que puede haber cambiado recientemente (verificar)

- **Requisitos legales/fiscales** (por ejemplo, **facturación electrónica, SII, TicketBAI/Veri_factu_** en España) pueden actualizarse por normativa. **Verifica en la fecha del proyecto** qué obligaciones tiene tu empresa según su comunidad autónoma y sector.
- **Modelos de licencia** y **precios** de ERPs SaaS cambian con frecuencia (límites de usuarios, almacenamiento, módulos). Comprueba condiciones **vigentes** antes de contratar.
- **APIs de terceros** (bancos, marketplaces, transportistas) modifican endpoints, autenticación o cuotas; revisa documentación **reciente**.

> Nota

> Los **principios de implantación** (fases, limpieza de datos, pruebas, formación) son estables. Lo que cambia son **leyes, tarifas y APIs.**

---
## 11) Errores comunes y cómo evitarlos

1. **Implantar sin objetivos medibles**

    - *Error*: “que todo vaya mejor”.  
    - *Solución*: 3–5 KPI claros desde el inicio (DSO, errores de factura, exactitud de stock).

2. **Querer personalizarlo todo el Día 1**

    - *Error*: tocar el núcleo del ERP.
    - *Solución*: **parametriza primero**; personalizaciones como **extensiones** y en **fases**.

3. **Olvidar la formación**

    - *Error*: usuarios perdidos, rechazo al sistema.
    - *Solución*: guías cortas + vídeos + “aula de guardia” la primera semana.

4. **No planificar el corte**

    - *Error*: convivir semanas con dos sistemas en paralelo sin control.
    - *Solución*: fecha y protocolo de “cut-over”; sólo un sistema “oficial”.

5. **Migrar datos sin limpiar**

    - *Error*: duplicates everywhere.
    - *Solución*: reglas de unicidad (NIF/email), normalización y validaciones.

6. **Subestimar pruebas integradas**

    - *Error*: cada módulo funciona “solo”, pero el flujo completo rompe.
    - *Solución*: simulacro de un día real antes del arranque.

7. **No cuidar permisos y auditoría**

    - *Error*: riesgos de seguridad y fraudes internos difíciles de rastrear.
    - *Solución*: roles, logs de cambios y revisiones periódicas.

---
## 12) Resumen

- Implantar un ERP es un **proyecto de negocio**, no sólo de TI.
- Empieza con **alcance claro**, **datos limpios**, **parametrización sensata** y **pruebas integradas**.
- **Arranca por fases** con KPI visibles y soporte cercano.
- Decide **SaaS vs. on-premise** mirando el **TCO** y el cumplimiento.
- El éxito depende del **factor humano**: comunicación, formación y gestión del cambio.
