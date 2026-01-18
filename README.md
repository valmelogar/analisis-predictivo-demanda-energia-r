# analisispredictivo
Informe predictivo demanda de energía
---
title: "Análisis predictivo"
output: html_notebook
date: "03-12-2025"
---

CARGA Y EXPLORACIÓN DE DATOS

```{r}
library(readr)
datos <- read_csv("datos.csv")
```

```{r}
dim(datos)       #Para ver las dimensiones de las filas y columnas
str(datos)       #Para ver tipos de datos
head(datos)
```
SELECCION DE VARIABLES

Para seleccionar variables para un modelo multivariado, primero medimos la relación lineal entre las predictoras y la variable objetivo que es demanda_energia

```{r}
#Matriz de correlación
matriz_cor <- cor(datos)

print("Matriz de Correlación:")
print(round(matriz_cor, 2))
```
Esta es la matriz de correlación de todos los elementos del dataset, ahora aislaremos la fila de nuestra variable objetivo

```{r}
cor_objetivo <- matriz_cor["demanda_energia", ] #Matriz de correlación de la variables con nuestra variable objetivo

print("Correlaciones con 'demanda_energia':")
print(cor_objetivo)
```
Visualizamos la matriz de correlación en un gráfico

```{r}
install.packages("corrplot")
library(corrplot)

#Gráfico de correlación
corrplot(matriz_cor, method = "number", type = "upper",
         title = "Matriz de Correlaciónn",
         mar = c(0,0,1,0))
```
```{r}
#Modelo 1: Regresión Lineal (RLM)
modelo_1 <- lm(demanda_energia ~ temperatura_media + consumo_historico + desempleo, data = datos)
modelo_1
```
```{r}
#Modelo 2 (RLM con interacción). Lo que hacemos aquí es añadir la interacción de que el efecto de la temperatura_media dependa del nivel de consumo_historico (ch).

modelo_2 <- lm(demanda_energia ~ temperatura_media + consumo_historico + desempleo +                 temperatura_media:consumo_historico, data = datos)
modelo_2
```
METRICAS DE EVALUACION

```{r}
#Usamos summary() para ver los coeficientes y la calidad del ajuste 
print("Resumen del Modelo 1")
summary(modelo_1)

print("Resumen del Modelo 2")
summary(modelo_2)
```

```{r}
#Usamos AIC y BIC para evaluar el modelo 1
aic_modelo_1 <- AIC(modelo_1)
bic_modelo_1 <- BIC(modelo_1)

print(paste("AIC:", round(aic_modelo_1,2)))
print(paste("BIC:", round(bic_modelo_1,2)))
```

```{r}
#Usamos AIC y BIC para evaluar el modelo 1
aic_modelo_2 <- AIC(modelo_2)
bic_modelo_2 <- BIC(modelo_2)

print(paste("AIC:", round(aic_modelo_2,2)))
print(paste("BIC:", round(bic_modelo_2,2)))
```

```{r}
#Tabla comparativa de los dos modelos
print("Comparación de Modelos (AIC/BIC)")
comparacion <- data.frame(
  Modelo = c("Modelo 1 (RLM)", "Modelo 2 (RLM Con Interacción)"),
  AIC = c(aic_modelo_1, aic_modelo_2),
  BIC = c(bic_modelo_1, bic_modelo_2)
)
print(comparacion)
```


