# Análisis de Comportamiento de Usuarios y Test A/A/B

## Descripción del Proyecto

Este proyecto analiza el comportamiento de usuarios en una aplicación de productos alimenticios mediante dos enfoques principales:

1. **Análisis del Embudo de Conversión**: Identifica cómo los usuarios navegan desde el inicio hasta la compra, detectando cuellos de botella y oportunidades de mejora.

2. **Test A/A/B**: Evalúa el impacto de un cambio en las fuentes de la aplicación mediante un experimento controlado con tres grupos (dos grupos de control y un grupo de prueba).

## Estructura del Proyecto

```
11. Sprint 11/
│
├── Proyecto_S11.ipynb          # Notebook principal con todo el análisis
├── logs_exp_us.csv              # Dataset con los eventos de usuarios
├── Tracking_plan_sample_us.xlsx # Plan de seguimiento (opcional)
└── README.md                    # Este archivo
```

## Dataset

### Archivo: `logs_exp_us.csv`

El dataset contiene registros de eventos de usuarios con las siguientes columnas:

- **EventName**: Nombre del evento (MainScreenAppear, OffersScreenAppear, CartScreenAppear, PaymentScreenSuccessful, Tutorial)
- **DeviceIDHash**: Identificador único de usuario
- **EventTimestamp**: Timestamp Unix del evento
- **ExpId**: ID del grupo experimental (246: Control A, 247: Control B, 248: Prueba)

### Características del Dataset

- **Período de análisis**: 1-7 de agosto de 2019 (7 días completos)
- **Total de eventos**: 241,298 eventos
- **Usuarios únicos**: 7,534 usuarios
- **Grupos experimentales**: 3 grupos balanceados (246, 247, 248)

## Requisitos

### Librerías Python

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime
from scipy import stats
from statsmodels.stats.proportion import proportions_ztest
```

### Instalación

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels
```

## Estructura del Análisis

El notebook está organizado en 5 pasos principales:

### Paso 1: Abrir el archivo de datos y leer la información general
- Carga del dataset CSV (separador: tabulador)
- Exploración inicial de los datos
- Verificación de dimensiones y tipos de datos

### Paso 2: Preparar los datos para el análisis
- Renombrado de columnas para mayor claridad
- Conversión de timestamps Unix a datetime
- Creación de columnas de fecha
- Verificación de valores ausentes y tipos de datos

### Paso 3: Estudiar y comprobar los datos
- Conteo de eventos y usuarios únicos
- Cálculo del promedio de eventos por usuario
- Análisis del período de tiempo cubierto
- Identificación y filtrado de datos incompletos
- Verificación de grupos experimentales

### Paso 4: Estudiar el embudo de eventos
- Análisis de frecuencia de eventos
- Identificación de usuarios por evento
- Determinación del orden lógico del embudo
- Cálculo de proporciones entre etapas
- Identificación de cuellos de botella
- Cálculo de tasa de conversión final

### Paso 5: Estudiar los resultados del experimento (Test A/A/B)
1. Conteo de usuarios por grupo experimental
2. Test A/A: Comparación entre grupos de control (246 vs 247)
3. Verificación de división correcta de grupos
4. Test A/B: Comparación del grupo de prueba (248) con controles
5. Análisis de nivel de significancia y corrección de Bonferroni

## Resultados Principales

### Análisis del Embudo de Conversión

- **Secuencia identificada**: MainScreenAppear → OffersScreenAppear → CartScreenAppear → PaymentScreenSuccessful
- **Tasa de conversión final**: 47.70% de usuarios completan el viaje hasta el pago
- **Cuello de botella principal**: 38.09% de pérdida en la primera transición (MainScreenAppear → OffersScreenAppear)
- **Alta retención en etapas finales**: 81.30% y 94.78% de retención en las últimas etapas

### Test A/A/B - Validación y Resultados

#### Validación de Grupos de Control (Test A/A)
- ✅ **Grupos validados correctamente**: No se encontraron diferencias significativas entre los grupos de control (246 vs 247)
- ✅ **Metodología confirmada**: La asignación aleatoria funcionó correctamente

#### Impacto del Cambio de Fuentes (Test A/B)
- ✅ **Sin impacto negativo**: No se encontraron diferencias significativas entre el grupo de prueba (248) y los grupos de control
- ✅ **Resultado robusto**: Las conclusiones se mantienen tanto con alpha=0.05 como con corrección de Bonferroni (alpha=0.0025)
- ✅ **Recomendación**: Es **SEGURO** implementar el cambio de fuentes

### Análisis Estadístico

- **Total de pruebas realizadas**: 20 pruebas estadísticas
  - Test A/A: 5 pruebas
  - Test A/B individual: 10 pruebas
  - Test A/B combinado: 5 pruebas
- **Nivel de significancia**: 0.05 (sin corrección) y 0.0025 (con corrección de Bonferroni)
- **Método estadístico**: Test de proporciones (z-test) usando `proportions_ztest` de statsmodels

## Cómo Ejecutar el Proyecto

1. **Asegúrate de tener los archivos necesarios**:
   - `logs_exp_us.csv` en el mismo directorio que el notebook

2. **Abre el notebook**:
   ```bash
   jupyter notebook Proyecto_S11.ipynb
   ```

3. **Ejecuta las celdas en orden**:
   - Las celdas están organizadas secuencialmente
   - Cada paso depende de los anteriores
   - Ejecuta desde el inicio hasta el final

4. **Revisa los resultados**:
   - Cada celda muestra outputs con análisis y visualizaciones
   - Las conclusiones finales están en la última celda markdown

## Metodología Estadística

### Test de Proporciones

Se utiliza el test z para comparar proporciones entre grupos:

```python
from statsmodels.stats.proportion import proportions_ztest

z_stat, p_value = proportions_ztest(count, nobs, alternative='two-sided')
```

Donde:
- `count`: Lista con el número de éxitos en cada grupo
- `nobs`: Lista con el número total de observaciones en cada grupo
- `alternative`: 'two-sided' para prueba bilateral

### Corrección de Bonferroni

Para múltiples comparaciones, se aplica la corrección de Bonferroni:

```
alpha_corrected = alpha_global / n_tests
```

Esto reduce la probabilidad de falsos positivos cuando se realizan múltiples pruebas estadísticas.

## Conclusiones

1. **El cambio de fuentes es seguro de implementar**: No se encontró evidencia de impacto negativo en el comportamiento de los usuarios.

2. **Oportunidad de mejora en el embudo**: La primera transición del embudo representa la mayor oportunidad de optimización.

3. **Metodología robusta**: El test A/A validó correctamente la metodología experimental, y el test A/B proporcionó resultados confiables.

## Autor

Proyecto desarrollado como parte del Sprint 11 del programa de análisis de datos.

## Licencia

Este proyecto es parte de un programa educativo.

