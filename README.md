# Análisis predictivo de demanda de energía con R

Este proyecto desarrolla un análisis predictivo de la demanda de energía utilizando modelos de regresión lineal en R.

El objetivo principal es estudiar la relación entre distintas variables explicativas, como la temperatura media, el consumo histórico y el desempleo, y una variable objetivo: la demanda de energía.

A partir del análisis de correlación y la comparación de modelos, se evalúa qué combinación de variables permite explicar mejor el comportamiento de la demanda energética.

## Contexto del proyecto

La predicción de la demanda de energía es una tarea relevante para empresas eléctricas, administraciones públicas y sistemas de planificación energética.

Conocer qué factores influyen en el consumo energético permite anticipar necesidades, optimizar recursos y mejorar la toma de decisiones.

En este proyecto se trabaja con un enfoque estadístico basado en regresión lineal múltiple, comparando un modelo base con otro modelo que incorpora un término de interacción entre variables.

## Objetivos

Los objetivos principales del proyecto son:

- Cargar y explorar un dataset de demanda energética.
- Analizar la relación entre las variables mediante una matriz de correlación.
- Seleccionar variables candidatas para un modelo predictivo.
- Construir modelos de regresión lineal múltiple.
- Comparar modelos utilizando métricas estadísticas.
- Interpretar los resultados obtenidos.
- Generar un informe reproducible en formato R Markdown.

## Dataset utilizado

El proyecto utiliza un archivo CSV llamado:

```text
datos.csv
```

El dataset contiene variables relacionadas con factores que pueden influir en la demanda de energía.

Las variables principales utilizadas en el análisis son:

- `demanda_energia`: variable objetivo que representa la demanda energética.
- `temperatura_media`: temperatura media registrada.
- `consumo_historico`: consumo energético histórico.
- `desempleo`: indicador socioeconómico utilizado como variable explicativa.

El análisis parte de la hipótesis de que estas variables pueden tener relación con la demanda de energía y, por tanto, pueden ser útiles para construir un modelo predictivo.

## Tecnologías utilizadas

- R
- R Markdown
- readr
- corrplot
- Regresión lineal múltiple
- AIC
- BIC

## Estructura del proyecto

```text
analisis-predictivo-demanda-energia-r/
│
├── analisis_predictivo_demanda_energia.Rmd
├── informe_predictivo_demanda_energia.pdf
├── datos.csv
└── README.md
```

## Flujo de trabajo

El proyecto se divide en las siguientes fases:

1. Carga del dataset.
2. Exploración inicial de los datos.
3. Análisis de dimensiones y estructura.
4. Cálculo de la matriz de correlación.
5. Análisis de correlaciones con la variable objetivo.
6. Visualización de la matriz de correlación.
7. Construcción de un modelo de regresión lineal múltiple.
8. Construcción de un modelo con interacción entre variables.
9. Evaluación de modelos mediante `summary()`, AIC y BIC.
10. Comparación final de resultados.

## Carga y exploración de datos

En primer lugar, se carga el dataset utilizando la librería `readr`.

```r
library(readr)

datos <- read_csv("datos.csv")
```

Después se revisa la estructura del dataset:

```r
dim(datos)
str(datos)
head(datos)
```

Esta fase permite conocer el número de filas y columnas, los tipos de datos disponibles y una primera muestra de los registros.

## Selección de variables

Para seleccionar variables candidatas para el modelo predictivo, se calcula una matriz de correlación.

La matriz de correlación permite analizar la relación lineal entre las variables del dataset.

```r
matriz_cor <- cor(datos)

print("Matriz de Correlación:")
print(round(matriz_cor, 2))
```

Posteriormente, se analiza específicamente la correlación de cada variable con la variable objetivo:

```r
cor_objetivo <- matriz_cor["demanda_energia", ]

print("Correlaciones con 'demanda_energia':")
print(cor_objetivo)
```

Este paso ayuda a identificar qué variables pueden tener mayor relación con la demanda energética.

## Visualización de correlaciones

Para facilitar la interpretación de las relaciones entre variables, se utiliza la librería `corrplot`.

```r
library(corrplot)

corrplot(
  matriz_cor,
  method = "number",
  type = "upper",
  title = "Matriz de Correlación",
  mar = c(0, 0, 1, 0)
)
```

La visualización permite detectar relaciones positivas, negativas o débiles entre las variables analizadas.

## Modelos predictivos

En el proyecto se construyen dos modelos de regresión lineal múltiple.

### Modelo 1: Regresión lineal múltiple

El primer modelo utiliza como variables predictoras la temperatura media, el consumo histórico y el desempleo.

```r
modelo_1 <- lm(
  demanda_energia ~ temperatura_media + consumo_historico + desempleo,
  data = datos
)
```

Este modelo permite evaluar el efecto individual de cada variable sobre la demanda de energía.

### Modelo 2: Regresión lineal con interacción

El segundo modelo incorpora una interacción entre `temperatura_media` y `consumo_historico`.

```r
modelo_2 <- lm(
  demanda_energia ~ temperatura_media + consumo_historico + desempleo + temperatura_media:consumo_historico,
  data = datos
)
```

La interacción permite analizar si el efecto de la temperatura media sobre la demanda energética cambia en función del nivel de consumo histórico.

## Evaluación de modelos

Los modelos se evalúan mediante la función `summary()`.

```r
summary(modelo_1)
summary(modelo_2)
```

Esta función permite analizar:

- Coeficientes estimados.
- Significancia estadística de las variables.
- Error estándar.
- Valor p.
- R-squared.
- Adjusted R-squared.
- Calidad general del ajuste.

Además, se calculan las métricas AIC y BIC para comparar ambos modelos.

```r
aic_modelo_1 <- AIC(modelo_1)
bic_modelo_1 <- BIC(modelo_1)

aic_modelo_2 <- AIC(modelo_2)
bic_modelo_2 <- BIC(modelo_2)
```

## Comparación de modelos

Para comparar los modelos, se crea una tabla resumen con los valores de AIC y BIC.

```r
comparacion <- data.frame(
  Modelo = c("Modelo 1: RLM", "Modelo 2: RLM con interacción"),
  AIC = c(aic_modelo_1, aic_modelo_2),
  BIC = c(bic_modelo_1, bic_modelo_2)
)

print(comparacion)
```

En general:

- Un AIC más bajo indica mejor equilibrio entre ajuste y complejidad.
- Un BIC más bajo penaliza más la complejidad del modelo.
- Si el modelo con interacción reduce AIC y BIC, puede considerarse una mejora.
- Si el modelo con interacción no mejora estas métricas, el modelo más simple puede ser preferible.

## Conclusiones

El proyecto muestra cómo aplicar un flujo básico de análisis predictivo con R para estudiar la demanda energética.

La matriz de correlación permite identificar relaciones iniciales entre variables, mientras que la regresión lineal múltiple permite cuantificar el efecto de cada predictor sobre la demanda de energía.

La comparación entre un modelo base y un modelo con interacción permite evaluar si añadir complejidad mejora realmente el ajuste del modelo.

Este tipo de análisis puede servir como punto de partida para sistemas de predicción más avanzados en el ámbito energético.

## Aprendizajes del proyecto

Durante este proyecto se han trabajado competencias clave de análisis estadístico y modelado predictivo:

- Carga de datos en R.
- Exploración inicial de datasets.
- Análisis de correlación.
- Visualización de matrices de correlación.
- Selección de variables predictoras.
- Regresión lineal múltiple.
- Modelos con interacción.
- Evaluación de modelos con `summary()`.
- Comparación mediante AIC y BIC.
- Creación de informes reproducibles con R Markdown.

## Posibles mejoras futuras

- Añadir una separación entre conjunto de entrenamiento y test.
- Evaluar el modelo con métricas predictivas como RMSE, MAE y R².
- Analizar residuos para comprobar supuestos del modelo lineal.
- Incluir gráficos de diagnóstico del modelo.
- Probar modelos más avanzados, como Random Forest o XGBoost.
- Añadir validación cruzada.
- Crear visualizaciones más completas de la demanda energética.
- Publicar el informe en formato HTML además de PDF.

## Cómo ejecutar el proyecto

1. Clonar el repositorio:

```bash
git clone https://github.com/valmelogar/analisis-predictivo-demanda-energia-r.git
```

2. Entrar en la carpeta del proyecto:

```bash
cd analisis-predictivo-demanda-energia-r
```

3. Abrir el archivo R Markdown:

```text
analisis_predictivo_demanda_energia.Rmd
```

4. Instalar las librerías necesarias en R:

```r
install.packages("readr")
install.packages("corrplot")
```

5. Ejecutar el documento desde RStudio usando la opción:

```text
Knit
```

Esto generará el informe final a partir del código y las explicaciones incluidas en el archivo `.Rmd`.
