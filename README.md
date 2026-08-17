
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

## 🗂️Resultados Tecnicos

  ## 🔍Limpieza de datos

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

## 🪡Modelado

Modelo estrella

Tabla de hechos facturación (FactFinanzasRaw) relacionada a las dimensiones de cuenta contable, centro de costo, escenario y calendario, adicionalmente se crearon las tablas de "Medidas", "Conceptos" para la matriz de estado de resultados, "PuenteUtilidad" para el Waterfall de utilidad y "UltimaActualizacion" para indicar en que fecha se actualizaron los datos del Dashboard (Muestra los datos existentes en las BBDD a dicha fecha)

<img width="1045" height="492" alt="Vista de Modelo" src="https://github.com/user-attachments/assets/64df9a37-02dd-4203-8ab7-830e75a54fb5" />

## 📐Medidas DAX

<img width="197" height="299" alt="Medidas" src="https://github.com/user-attachments/assets/a2e0dd03-8828-4971-9529-478860de6a61" />

Se crearon 168 medidas agrupadas por carpetas para una mejor organización y visualización de datos 

## 📊📈Dashboard

Datos del periodo 2025

<img width="820" height="520" alt="Dashboard 2025" src="https://github.com/user-attachments/assets/38fa7653-5a28-44cf-bc4d-3ad42672a68a" />

Datos del periodo 2024

<img width="825" height="520" alt="Dashboard 2024" src="https://github.com/user-attachments/assets/12a264e7-6021-48c7-a543-57c0aaf984a2" />

Variación 2025 vs 2024

<img width="336" height="301" alt="Variación año anterior" src="https://github.com/user-attachments/assets/ad17c66f-0839-4c3b-9cd3-1456af759aaa" />

---

## 📊Resultados Financieros 

  ## Tarjetas KPI´s a destacar 

  <img width="1146" height="169" alt="Tarjetas KPI´s" src="https://github.com/user-attachments/assets/60d44f08-7b8c-450a-b243-aae051a32259" />

- Margen bruto de 71% esto es un indicativo muy bueno para un negocio donde sus costos directos representan solo el 29% de la venta generando gran utilidad

- EBITDA de 4MILL con una venta de 8MILL esto excelente la empresa tiene capacidad para cubrir aumento de costos o caída en venta

- Utilidad Neta de 4MILL es decir, la depreciación, intereses y amortización no representan gran impacto dentro del resultado

    ## Gastos por centro de costo 

<img width="351" height="189" alt="Gastos por centro de costo" src="https://github.com/user-attachments/assets/a6cb0d58-83f9-4f01-bec8-1496c195cb3d" />

- El centro de costo con mayor gasto es "Tecnología" con 0,84MILL
  
- Los centro de costo con menor gasto son "Administración y Dirección con 0,58MILL
  
- Las variaciones entre centro de costo se muestran prudente y equilibradas en relación a la venta 

    ## Evolución mensual de ingresos

<img width="491" height="217" alt="Evolución mensual" src="https://github.com/user-attachments/assets/613ae914-0811-458e-bab2-5c81ba6e8b71" />

- Los ingresos mantienen un comportamiento similar sin variaciones abruptas
  
- Hay que destacar las caída mas significativas en mayo, junio y diciembre, bajo los 0,6 mill. Pero es importante mencionar que puede ser por estacionalidad debido a que tanto junio como diciembre están mapeadas así en presupuesto
  
- Enero presenta una caída significativa vs presupuesto pero con crecimiento vs 2024

  ## Estado de resultado
  
<img width="660" height="200" alt="P L" src="https://github.com/user-attachments/assets/514e31f0-6b82-4b2f-b5bc-335784cf4f65" />

-  A pesar de que ninguna línea del P&L cumple con el presupuesto asignado, la salud financiera del negocio es excelente
  
-  Cuenta con excelente margen, EBITDA y Utilidad
  
-  Revisando las variaciones vs 2024 también se detecta que hay crecimiento en ventas y margen bruto y a pesar de que el EBITDA y Utilidad disminuyen 1 punto aproximadamente cada uno, la salud financiera sigue siendo muy buena

## 📍Insights clave del negocio

- Crecimiento vs 2024 en venta +3,6% pero caída en EBITDA -0,6% y utilidad Neta -1,2%, a pesar de generar mayores ingresos estos se ven absorbidos casi en su totalidad por mayor costo, sin embargo la salud financiera continua siendo buena

- El presupuesto fue extremadamente optimista esperando un crecimiento de más de 5% en utilidad pero las ventas no alcanzaron la meta y los costos por el contrario fueron mayores.

- Los sobregastos más significativos fueron en los centros de costos de Tecnología (+29%) y seguido de marketing (+30%)
 
---

## 📋Conclusiones

La empresa está vendiendo más que el año pasado, pero gastando más rápido de lo que factura — principalmente por Tecnología que puede estar fuertemente relacionado con el crecimiento tecnológico a nivel mundial y ninguna empresa debería quedarse atrás en esta materia y seguido de Marketing que a pesar de la mayor inversión no logró retribuir con el incremento de ventas esperado para compensar este sobregasto; El presupuesto de ventas 2025 fue poco realista, lo que infla artificialmente la sensación de "mal desempeño" cuando en realidad el problema de fondo está en el control de gastos operativos y no en las ventas.

---

## 💭Recomendaciones

- Revisar a fondo gastos en Tecnología y Marketing (Gastos no presupuestados, relación costo/beneficio, si es gasto recurrente que se debe agregar al 2026 o esta relacionado a un proyecto específico)
  
- Replantear supuestos utilizados para la elaboración del presupuesto 2025

- Crear escenarios de sensibilidad que permitan revisar si estamos frente a un mal pronóstico real o frente a un escenario poco realista.





