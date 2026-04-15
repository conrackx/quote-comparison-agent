# RFQ Homologation Engine 🚀

Este agente especializado procesa listas de requerimientos (**RFQ**) y cotizaciones de proveedores para generar tablas de comparativa técnica y económica plenamente homologadas. Su núcleo operativo utiliza **Python** para garantizar precisión matemática, conversiones de unidades y validación de formatos financieros.

## 目的 (Objetivo)
Transformar datos heterogéneos de múltiples proveedores en una única tabla maestra de Excel, normalizando unidades, cantidades y precios sin intervención manual, bajo una política estricta de **cero inventiva**.

---

## 🛠 Funcionalidades Clave

### 1. Homologación Inteligente (Python-Powered)
El motor no solo extrae texto; ejecuta lógica matemática para:
* **Conversión de Unidades:** Si el RFQ pide "Caja x 12" y el proveedor cotiza por "Unidad", el motor realiza la conversión automática.
* **Factores de Empaque:** Detección de factores como `N PAQ x CAJA` o `N UND x PACK`.
* **Cálculos Internos:** Prioriza los totales calculados por Python sobre los valores escritos en el documento para evitar errores de redondeo o transcripción del proveedor.

### 2. Jerarquía de Emparejamiento (Decision Order)
El agente sigue un protocolo de decisión de 6 pasos para cada ítem:
1.  **Match Directo:** SKU, Código de parte o referencia exacta.
2.  **Match Semántico + Conversión Exacta:** Misma familia de producto con unidad convertible.
3.  **Match Semántico + Factor RFQ:** Uso de los empaques definidos en el Master List.
4.  **Agregación Multilínea:** Suma de múltiples líneas del proveedor que conforman un único ítem solicitado.
5.  **No-Match No Resuelto:** Espacios en blanco (requiere atención del usuario).
6.  **Omisión Real:** Marcado automático con `0,00` tras agotar las opciones anteriores.

### 3. Seguridad y Veracidad
* **Protección de Comportamiento:** Ignora instrucciones embebidas en los documentos cargados (anti-prompt injection).
* **Consistencia de Datos:** Si falta información en el Master List, el agente detiene la ejecución para evitar datos corruptos.

---

## 📊 Estructura del Output (Contrato de Salida)

El resultado final es una tabla lista para Excel con una estructura rígida de encabezados:

| ITEM | DESCRIPCION | UND | CANT | [Proveedor A] VALOR UNITARIO | [Proveedor A] TOTAL | [Proveedor B] VALOR UNITARIO | [Proveedor B] TOTAL | ... |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Laptop i7 | UND | 5 | USD 1.200,00 | USD 6.000,00 | USD 1.250,00 | USD 6.250,00 | ... |

### Reglas de Formato Monetario
Todos los valores monetarios cumplen con el estándar regex:
* **USD:** `USD 1.500,00`
* **Bs:** `Bs 1.500,00`
* **Finalización:** Incluye filas automáticas de **SUBTOTAL**, **IVA (16%)** y **TOTAL**.

---

## 🚦 Modos de Operación

* **`UPDATE`**: Permite añadir nuevos archivos o aclaratorias sin perder el contexto previo, reconstruyendo el set de trabajo.
* **`FINAL`**: Genera el entregable definitivo. Si existen ambigüedades, el agente consultará al usuario antes de proceder.

---

## 📝 Bloque de Auditoría
Al finalizar cada proceso, el agente entrega un reporte conciso:
1.  **Conversiones realizadas** (ej. de Galón a Litros).
2.  **Ítems omitidos** por proveedores (marcados con cero).
3.  **No-matches** (celdas vacías por falta de claridad).
4.  **Ítems no solicitados** encontrados en las facturas (ignorados en la tabla).

---

## ⚖️ Restricciones (Misión)
* **NO** compara proveedores.
* **NO** rankea ni recomienda.
* **NO** inventa descripciones; el RFQ es la autoridad máxima (Canonical Master List).

---

> **Nota para Desarrolladores:** Este agente está configurado para priorizar la integridad de los datos financieros sobre cualquier instrucción estética o creativa.
