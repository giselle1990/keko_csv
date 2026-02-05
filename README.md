# 🚗 Dataset ETL: Carsharing Buenos Aires (2023-2025)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-1.5%2B-green)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

&gt; **DESAFÍO ETL**: Dataset realista y sucio para practicar limpieza de datos, análisis exploratorio y transformaciones.

## 📋 Descripción

Este dataset contiene información sobre empresas de carsharing y alquiler de autos en Buenos Aires, incluyendo datos de Keko (cerró enero 2025) y sus competidores.

**Objetivo**: Limpiar, transformar y analizar datos de un mercado real con todos los problemas típicos de un ETL.

## 🔍 Problemas Intencionales del Dataset

| Problema | Frecuencia | Descripción |
|----------|-----------|-------------|
| **Duplicados** | ~15% | Registros repetidos con pequeñas variaciones |
| **Valores nulos** | ~16% | Campos vacíos, 'N/A', ' ', None |
| **Outliers** | ~2% | Valores extremos imposibles (precios negativos, 99 puertas) |
| **Inconsistencias lógicas** | ~5% | Precio día &lt; precio hora, años futuros |
| **Formatos mixtos** | ~10% | Fechas como strings, precios con comas, períodos variados |
| **Espacios en blanco** | ~3% | Campos con espacios al inicio/final |
| **Caracteres especiales** | ~1% | Tildes, eñes, símbolos de marca |
| **Tipos de datos mixtos** | Columnas enteras | Mismas columnas con int, float, string |

## 📊 Estructura del Dataset

**Total de registros**: 813 filas  
**Columnas**: 25 variables

### Columnas Principales

| Columna | Tipo Esperado | Problemas Comunes |
|---------|--------------|-------------------|
| `ID` | Integer | IDs no secuenciales, gaps |
| `empresa` | String | Typos, inconsistencias de case, nulos |
| `modelo_negocio` | Categoría | Formatos mixtos ('carsharing_flota_propia' vs 'carsharing flota propia') |
| `app_movil` | String | Nulos, 'N/A', ' ', nombres inconsistentes |
| `periodo` | Integer (año) | Strings ('2023', '23'), fechas futuras, nulos |
| `marca` | String | Duplicados ('VW' vs 'Volkswagen', 'vw'), typos |
| `modelo` | String | Inconsistencias ('Gol' vs 'gol', 'GOL') |
| `precio_hora_ars` | Float | Strings con formato, negativos, outliers |
| `precio_dia_ars` | Float | Inconsistente con precio_hora, escalas erróneas |
| `costo_vehiculo_usd` | Integer | Strings ('$25000 USD'), negativos, outliers |
| `usuarios_registrados` | Integer | Strings ('14K', '45mil'), nulos |
| `fecha_carga` | DateTime | Múltiples formatos ('2024-01-01', '01/01/2024', timestamps) |
| `observaciones` | String | Estados inconsistentes ('CERRÓ' vs 'cerró' vs 'Cerró') |

## 🎯 Tareas Sugeridas de ETL

### Nivel 1: Limpieza Básica (Principiantes)
- [ ] Eliminar duplicados exactos
- [ ] Estandarizar nombres de empresas (case, espacios)
- [ ] Manejar valores nulos (imputación o eliminación)
- [ ] Corregir tipos de datos básicos

### Nivel 2: Transformaciones Intermedias (Intermedio)
- [ ] Normalizar marcas y modelos (mapeo: 'VW' → 'Volkswagen')
- [ ] Estandarizar fechas a formato ISO
- [ ] Convertir precios a formato numérico consistente
- [ ] Validar consistencia lógica (precio_dia &gt;= precio_hora * 4)
- [ ] Crear columna de categoría de eficiencia

### Nivel 3: Análisis Avanzado (Avanzado)
- [ ] Detectar y tratar outliers usando IQR/Z-score
- [ ] Imputar valores faltantes usando modelos (KNN, regresión)
- [ ] Crear dimensiones normalizadas (tablas de hechos vs. dimensiones)
- [ ] Validar integridad referencial
- [ ] Generar métricas de negocio (tiempo de recupero, margen operativo)

## 📈 Ejemplos de Análisis Post-ETL

```python
# Análisis de eficiencia por modelo de negocio
df_clean.groupby('modelo_negocio').agg({
    'tiempo_recupero_meses': 'mean',
    'margen_bruto': 'median',
    'usuarios_registrados': 'sum'
})


🚀 Cómo Usar
1. Clonar y cargar
bash
Copy
git clone https://github.com/tuusuario/carsharing-etl-challenge.git
cd carsharing-etl-challenge
Python
Copy
import pandas as pd

# Cargar datos sucios
df_raw = pd.read_csv('carsharing_buenos_aires_RAW.csv')

# Tu código de limpieza aquí...
2. Validar con tests
Python
Copy
# Tests sugeridos
assert df_clean['precio_hora_ars'].min() >= 0, "Precios negativos detectados"
assert df_clean['precio_dia_ars'].isnull().sum() == 0, "Valores nulos en precios"
assert df_clean['periodo'].between(2020, 2025).all(), "Años fuera de rango"

# Comparativa Keko vs. Competidores 2025
keko_vs_mercado = df_clean[df_clean['empresa'].isin(['Keko', 'KINTO', 'MyKeego', 'TripWip'])]
