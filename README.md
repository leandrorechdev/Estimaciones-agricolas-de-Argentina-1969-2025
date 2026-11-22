# 🌾 Estimaciones Agrícolas de Argentina (1969–2025)

Análisis descriptivo, exploratorio y predictivo de **56 campañas agrícolas en Argentina** (departamento por departamento), acompañado por un tablero interactivo web que replica todo el pipeline de limpieza y visualización.

🔗 **Demo interactiva en vivo:** [hexagonx.io/tpi/#/panorama](https://hexagonx.io/tpi/#/panorama)  
📓 **Notebook de análisis:** [`notebooks/estimaciones_agricolas_argentina.ipynb`](notebooks/estimaciones_agricolas_argentina.ipynb) *(puedes abrirlo directamente en Google Colab desde el botón superior del archivo)*

---

## 🎯 Pregunta guía del proyecto
> *“¿Cómo evolucionó la producción agrícola argentina desde 1969, dónde se concentra y en qué medida se puede anticipar el rendimiento de cada departamento?”*

El proyecto sigue un ciclo inspirado en **CRISP-DM** dividido en cinco etapas reproducibles de principio a fin:
1. **Definición y comprensión del problema:** Alcance, preguntas analíticas y auditoría inicial del dataset oficial.
2. **Limpieza y procesamiento trazable:** Reglas explícitas de calidad sin borrar registros en silencio.
3. **Análisis exploratorio (EDA):** Evolución histórica, concentración territorial y volatilidad interanual de rindes.
4. **Modelado predictivo:** Estimación del rendimiento departamental de soja con validación libre de fuga de datos (*data leakage*).
5. **Visualización y producto web:** Tablero interactivo con filtros globales por campaña, cultivo, región, provincia y departamento.

---

## 📊 Dataset y Pipeline de Limpieza

* **Fuente:** Secretaría de Agricultura, Ganadería y Pesca — [datos.gob.ar](https://datos.gob.ar)
* **Cobertura:** 56 campañas agrícolas (`1969/70` a `2024/25`), 33 cultivos, 506 departamentos en 23 provincias.
* **Principio de trazabilidad:** Ningún dato se modifica sin rastro; cada corrección conserva el valor original o añade una bandera de auditoría.

| Control aplicado | Registros | Tratamiento |
| :--- | :---: | :--- |
| **Filas crudas originales** | 160.499 | Dataset base importado desde `datos.gob.ar` |
| **Duplicados exactos y por clave** | 0 | Verificado sin duplicados |
| **Rindes con ceros de más** | 4 | Corregidos (`÷ 10`) según umbral por cultivo |
| **Inconsistencia Producción ≠ Superficie × Rinde** | 15 | Recalculadas (16.824 casos menores eran solo redondeo) |
| **Superficie cosechada > sembrada** | 11 | Marcadas con bandera de auditoría sin eliminar |
| **Filas sin provincia ni departamento** | 8 | Marcadas; se conservan para el total nacional |
| **Subcategorías con doble conteo** | 24.224 | Excluidas (se conservan totales históricos de soja, trigo, cebada y poroto) |
| **Dataset final de análisis** | **136.275** | **33 cultivos · 56 campañas listas para análisis y modelado** |

> 💡 **Hallazgo clave de calidad:** Conservar simultáneamente totales y subcategorías inflaba la producción acumulada en **1.164 millones de toneladas** (5.519 M t brutas frente a **4.355 M t** reales).

---

## 🔍 Principales Resultados

* **Crecimiento sostenido con impacto climático:** La producción creció a una tasa anual compuesta de **+2,38 %** entre 1969/70 y 2024/25, alcanzando un récord de **148 M t** en `2018/19` y **143 M t** en `2024/25`, pero con caídas superiores al 15 % en campañas de sequía severa (`2008/09` y `2022/23`, donde el rinde de soja cayó **−37,4 %** frente a su tendencia).
* **Alta concentración territorial:** El **75,5 %** del volumen producido en las últimas 5 campañas proviene de solo tres provincias (**Buenos Aires, Córdoba y Santa Fe**), con un índice de concentración **HHI de 2.211**.
* **Predicción de rinde departamental (Soja, kg/ha):**
  * *Escenario A (Anticipado precosecha):* Sin variables meteorológicas de la campaña en curso, los modelos complejos no superan a una línea base histórica (`MAE ≈ 541–545 kg/ha`).
  * *Escenario B (Contexto espacial intraprovincial):* Incorporando el desempeño de los departamentos vecinos en la misma campaña (sin fuga de datos), la **Regresión Ridge** reduce el error cerca de un **40 %** (`MAE = 326 kg/ha`, `R² = 0,65`), superando a Random Forest, Gradient Boosting y al promedio provincial.

---

## 🛠️ Stack Tecnológico

* **Análisis y Ciencia de Datos:** Python, Pandas, NumPy, Scikit-learn, Google Colab
* **Tablero Interactivo (`hexagonx.io/tpi`):** PHP, SQLite, Apache ECharts, Leaflet (límites geográficos INDEC) y asistente analítico guiado

---

## 👥 Autores

Trabajo Práctico Integrador — **Introducción a la Ciencia de Datos · Universidad del Gran Rosario (UGR) · 2026**

* **Leandro Rech**
* **Agustín Berardi**
* **Lucas Cuneo**
* **Ezequiel Cianchetta**
