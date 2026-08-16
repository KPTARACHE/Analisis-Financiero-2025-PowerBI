
# 📈📊Análisis Financiero Período 2025.

|Dasboard Ejecutivo - Power Query M - Modelado de datos - DAX 

---

## 💻Stack Tecnológico

| Herramienta | Función | Lenguaje / Formato |
| :--- | :--- | :---: |
| **Power BI** | Visualización y Dashboards | .pbix |
| **Power Query** | Extracción, transformación y carga (ETL) | M|
| **Modelado y Medidas** | Creación de cálculos, relación y Texto ejecutivo | DAX |

---

## 📑Contexto del negocio

Empresa no cumplió con el presupuesto anual del periodo 2025 por cual necesita un análisis financiero que permita identificar las posibles causas para la toma de deciciones de la gerencia. 

  ⚠️Principales problemas de datos:
  
    - Calidad de base de datos (Celdas vacías, formatos inválidos o inconsistente, datos no normalizados, ID inexistente.

    - Posterior a la limpieza quedaron 3.176 filas de 3.452 lo que significa una perdida de del 8% de los datos.

  🎯💡Objetivo del proyecto y plan de acción
  
🎯El objetivo principal del proyecto es elaborar un Dashboard que permita visualizar de forma clara y eficiente el resultado financiero del periodo 2025 y compararlo vs presupuesto y año anterior para identificar posibles factores que interfirieron en el no cumplimiento del presupuesto.
    
💡 Para lograr el objetivo se ejecutó lo siguiente:

    - Carga de BBDD a Power BI
    - Limpieza de datos en Power Query
    - Creación de tabla calendario y medidas DAX
    - Elaboración de Dashboard
    - Análisis financiero de resultado
    - Resultados
    - Conclusión y recomendaciones
    
---

## Resultados

  ## Limpieza de datos

Problemas detectados por columnas.

🗓️Fecha

| Problema | Descripción |
| :--- | :--- |
| **Celdas vacías** | Blancos reales que requerían "Rellenar hacia abajo". |
| **Fechas inválidas** | 31/02/2025 (febrero no tiene 31 días) y 00/01/2024 (día "00" no existe). |
| **Texto en vez de fecha** | Valor literal "sin fecha" en algunas celdas. |
| **Formato inconsistente** | Mezcla de separadores `/` y `-` (curiosamente, el `/` coincidía siempre con las fechas inválidas). |
| **Tipo de dato incorrecto** | Toda la columna venía como Texto, no como Fecha. |
| **Bug de proceso #1** | El paso "Rellenar hacia abajo" quedó ejecutándose antes de reemplazar los valores malos por blanco — por eso esas celdas nunca se llenaban. |
| **Bug de proceso #2** | Al reemplazar valores por "vacío", Power Query los dejó como texto vacío `""` en vez de `null` real — y "Rellenar hacia abajo" solo actúa sobre `null`. |

💰Monto

| Problema | Descripción |
| :--- | :--- |
| **Formatos mixtos** | Números planos, `$37,627.11` (USD), `USD 37,517`, `37.627,11` (formato latino), `(2,460.62)` (negativo contable con paréntesis). |
| **Texto no numérico** | `"N/C"` y `"pendiente"` en vez de un monto. |
| **Bug de conversión #1** | La lógica inicial asumía que cualquier coma era separador de miles — números como `5258,68` (decimal) se inflaban ~100x a `525868`. |
| **Bug de conversión #2** | Cuando Excel ya traía el valor como número real (no texto), `Text.From()` lo convertía arrastrando ruido de punto flotante (`41925,979999999996`) que la lógica de texto interpretaba como separador de miles — inflando algunos montos a cuatrillones. |

📚Escenario

| Problema | Descripción |
| :--- | :--- |
| **Categorías inconsistentes** | `Ppto`, `Presup` y `Presupuesto` referían a lo mismo. |

🔑ID_

| Problema | Descripción |
| :--- | :--- |
| **CuentaID vacío** | Filas sin cuenta contable asignada. |
| **CentroCostoID vacío** | Filas sin centro de costo asignado. |
| **CuentaID inexistente** | 38 filas con `Cta_9999`, un código que no existe en `DimCuentas`. |
| **Moneda vacía** | Filas sin moneda asignada`"USD"` |

⚠️Impactos post limpieza y confiabilidad del dato

| Motivo de exclusión | Cantidad de filas | Impacto en monto USD |
| :--- | :---: | ---: |
| **Monto inválido ("N/C" o "pendiente")** | 149 | — *(sin valor numérico)* |
| **CentroCostoID vacío** | 47 | $134.661,72 |
| **CuentaID inexistente (Cta_9999)** | 38 | $13.574,37 |
| **CuentaID vacío** | 42 | $59.033,04 |
| **Total** | **276** | **$207.269,13** |

•	Impacto en Monto de lo eliminado: $207.269,13 USD (sobre los 127 registros donde el monto era identificable; 149 no tenían valor numérico disponible por venir marcados "N/C" o "pendiente")

---

## Modelado

Modelo estrella

Tabla de hechos facturación relacionada a las dimensiones de cuenta contable, centro de costo, escenario y tabla de calendario, adicionalmente se crearon las tablas de medidas, conceptos para la matriz de estado de resultados, PuenteUtilidad para el Waterfall de utilidad y UltimaActualizacion para indicar en que fecha se actualizo el Dashboard

<img width="1045" height="492" alt="Vista de Modelo" src="https://github.com/user-attachments/assets/64df9a37-02dd-4203-8ab7-830e75a54fb5" />


## Medidas DAX

## Dashboard

## Insight claves del negocio

## Visualizaciones

## Conclusiones

## Recomendaciones





