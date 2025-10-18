# Análisis de la efectividad de las intervenciones para la desvinculación del trabajo infantil en Colombia

**Autores de la presentación:** Diego Andres Álvarez, Janis Rodriguez Oyola, María Camila Infante  
**Última actualización:** 2025-10-16

---

## 1) Propósito del proyecto

- **Objetivo general:** Evaluar la efectividad del programa de desvinculación del trabajo infantil mediante el análisis integral de las intervenciones, los perfiles profesionales y las características de la población atendida.
- **Objetivos específicos:**
  1. **Efectividad por intervención/perfil**: Medir tasas de desvinculación y priorizar combinaciones con mayor impacto.
  2. **Mapa de riesgo por zona**: referenciar riesgo donde coexisten alta prevalencia y menor efectividad; entregar un *ranking* de zonas críticas.
  3. **Segmentación (clustering)**: identificar grupos de atención (perfiles/zonas) con necesidades diferenciales de intensidad o tipo de intervención.

---

## 2) Datos y preparación

- **Fuente**: Base administrativa anonimizada del sistema de atención a NNA vinculados o en riesgo de trabajo infantil en Colombia. Contiene registros de **intervenciones**, **seguimientos** y **perfiles profesionales** asociados al proceso de desvinculación.  
- **Muestra final**: 4.661 registros (tras limpieza).  
- **Diagnóstico inicial** (scripts de EDA): valores faltantes, tipos de datos, duplicados/constantes, distribución de casos por perfiles, zonas e intervenciones.
- **Variables clave** (post-limpieza y derivadas):
  - `desv_flag` (binaria): indicador de desvinculación (1 sí, 0 no).  
  - `n_seguimientos` (entero): número de seguimientos válidos.  
  - `interv_mes` (categórica YYYY-MM): mes de la intervención.  
  - `perfil` (categórica): perfil profesional del interventor (sin 99999).  
  - `localidad_fic` (categórica): localidad/municipio de ejecución.  
  - `edad` (numérica).  
  - `estrato` (numérica, si disponible).

---

## 3) Metodología analítica

1. **Efectividad global y por perfil/ intervención**
   - Cálculo de **tasa de desvinculación** global y desagregada por `perfil`, `interv_mes`, `localidad_fic`.
   - Regla de decisión: reportar solo combinaciones con tamaño muestral suficiente.

2. **Riesgo por zona**
   - Definición: `Riesgo = Prevalencia × (100 − Tasa de desvinculación)`.
   - Entregable: tabla interpretable + *ranking* de zonas críticas para focalización.

3. **Clustering de población atendida**
   - Variables: `n_seguimientos`, `edad`, `perfil`, `localidad_fic`, `interv_mes` (codificadas).  
   - Transformación: *one-hot* para categóricas; estandarización para numéricas.  
   - Reducción/diagnóstico: PCA 2D para inspección de separabilidad.  
   - Selección de `k`: por estabilidad y separación visual.
   - Interpretación: etiquetado operativo de clústeres dominados por perfiles/zonas.

---

## 4) Principales hallazgos (para orientar la toma de decisiones)

- **Tasa global de desvinculación:** ~33,4% (≈ 1 de cada 3 NNA se desvincula completamente).  
- **Perfiles profesionales:**
  - Tecnólogo en Salud Ocupacional: ~36% (mayor efectividad y mayor cobertura).
  - Trabajador Social: ~26,6%.
  - Otros perfiles (psicología, nutrición, enfermería) con baja participación y sin casos exitosos reportados en la muestra disponible.
- **Distribución temporal de intervenciones:** mayor concentración entre **dic-2024** y **mar-2025**, con picos en enero.
- **Zonas destacadas por efectividad:** Usme (~72%), Engativá (~70%), Suba (~68%).  
- **Zonas con mayor prevalencia (y riesgo relativo):** Ciudad Bolívar (~14%), Bosa (~10%), Kennedy (~9%).  
- **Clustering (8 grupos):**
  - Dominio de dos perfiles: Tecnólogos en Salud Ocupacional (clusters 0–3) y Trabajadores Sociales (clusters 4–7).
  - Zonas críticas frecuentes: Bosa y Ciudad Bolívar.
  - **Cluster 2:** ~81,6% de efectividad (posible referente operativo).
  - **Clusters 1 y 6:** 0% de efectividad (priorizar refuerzos de seguimiento/calidad).

> **Conclusión ejecutiva:** La efectividad depende más del **perfil profesional** y el **contexto territorial** que de la **frecuencia** de seguimiento; la **calidad** del seguimiento emerge como factor determinante.
