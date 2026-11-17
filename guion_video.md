# Guion del vídeo — qué decir en cada celda

**Duración:** 8–9 minutos (máximo 10).  
**Cómo usarlo:** abre `tarea.ipynb` ejecutado y ve bajando celda a celda. En cada **celda de código** di el texto de “Hablar”. Si la celda es solo técnica, di poco y pasa.

**Leyenda**
- **Hablar:** lo que dices en voz alta.
- **Pantalla:** qué señalar o enseñar.
- **Paso rápido:** no te pares; 5–10 segundos.

---

## Antes de la primera celda de código

**Pantalla:** título del cuaderno (markdown inicial).

**Hablar:**
> Hola, soy [tu nombre]. Este es mi proyecto completo de Machine Learning. Predigo el gasto turístico mensual de los residentes en España por país de destino, con datos del INE. Es un problema de regresión supervisada y la métrica principal es el RMSE.

---

## 1. Obtención de datos

### Celda 1 — imports (`numpy`, `pandas`, `matplotlib`…)

**Paso rápido.**

**Hablar:**
> Cargo las librerías habituales: pandas para la tabla, matplotlib y seaborn para gráficas, y más adelante scikit-learn para los modelos.

---

### Celda 2 — cargar `58692.csv`

**Pantalla:** las primeras filas del dataframe.

**Hablar:**
> Aquí cargo el CSV del INE, separado por punto y coma. Renombro las columnas a nombres más claros: país, tipo de gasto, periodo y total. La fuente es la tabla 58692: estadística oficial, no un sensor ni una encuesta mía.

---

### Celda 3 — tamaño del dataset

**Pantalla:** los `print` (filas, países, periodos).

**Hablar:**
> Tenemos unas cuarenta y cuatro mil filas, ciento sesenta y cinco países, tres magnitudes y noventa meses, de enero de 2019 a junio de 2026.

---

## 2–3. Descripción y exploración

*(El markdown de descripción: pásalo despacio 10–15 s y di: “Estas cuatro columnas son país, tipo de magnitud, mes e importe; el importe viene en texto con el formato del INE.”)*

### Celda 4 — `value_counts` de `tipo_gasto`

**Hablar:**
> Las tres series tienen el mismo número de filas: gasto turístico total, gasto medio diario y gasto con tarjeta. El fichero está en formato largo: tres filas por cada país y mes.

---

### Celda 5 — función `euros_ine` y conversión a número

**Pantalla:** columna `euros` junto a `total`.

**Hablar:**
> Convierto el texto del INE a número. El gasto medio usa coma decimal; el total y la tarjeta usan punto de miles. El punto solo o la celda vacía son datos no publicados, no ceros.

---

### Celda 6 — ejemplo Albania 2026M06

**Paso rápido.**

**Hablar:**
> Compruebo con Albania en junio de 2026: el total se lee en millones y el gasto medio diario en cientos de euros. La conversión es correcta.

---

### Celda 7 — pivot a formato ancho

**Pantalla:** columnas `gasto_turistico`, `gasto_medio_diario`, `gasto_tarjeta`.

**Hablar:**
> Paso a formato ancho: una fila por país y mes, con una columna para cada magnitud. También saco año, mes y fecha. Hay catorce mil ochocientas cincuenta combinaciones; unas nueve mil quinientas tienen el gasto publicado.

---

### Celda 8 — `describe()`

**Hablar:**
> Las escalas no se parecen. El gasto total tiene mediana de unos uno o dos millones y media mucho más alta: unos pocos destinos grandes estiran la media. El gasto medio diario vive en otra escala, alrededor de cien a doscientos cincuenta euros.

---

### Celda 9 — nulos por magnitud y por año

**Pantalla:** totales de huecos y tabla por año.

**Hablar:**
> Hay más de cinco mil países-mes sin cifra. Los tres huecos coinciden: o el INE publica las tres magnitudes o no publica ninguna. En 2020 falta cerca del 64 % y en 2021 la mitad; son los años de restricciones.

---

### Celda 10 — gráfica de huecos por año

**Hablar:**
> Esta barra lo deja claro: 2020 y 2021 rompen la serie. Esos huecos no se rellenan con una mediana: no hay un gasto típico válido para todos los destinos.

---

### Celda 11 — asignar región / continente

**Paso rápido.**

**Hablar:**
> Añado una región continental solo para explorar. El modelo más adelante usará el país, no la región, porque dentro de Europa conviven destinos muy distintos.

---

### Celda 12 — histogramas de las tres magnitudes

**Pantalla:** los tres paneles.

**Hablar:**
> El primer histograma corta el eje en veinte millones para que se vea la mayoría de países. El del medio, en logaritmo, enseña la cola grande. El tercero es el gasto medio diario, mucho más concentrado.

---

### Celda 13 — top 12 destinos por mediana

**Hablar:**
> Estos son los destinos con mayor gasto mensual mediano. Unos pocos países concentran casi todo el volumen.

---

### Celda 14 — serie temporal del gasto total

**Hablar:**
> Sumando todos los destinos se ve la onda estacional: cada verano sube. La franja roja marca 2020 y principios de 2021, cuando el gasto se hunde.

---

### Celda 15 — top 5 destinos en el tiempo

**Hablar:**
> Los cinco destinos grandes repiten el mismo dibujo año tras año, cada uno a su nivel. Esa repetición es lo que un modelo puede aprender.

---

### Celda 16 — mediana por mes (total vs diario)

**Hablar:**
> En agosto el gasto total es máximo, pero el gasto medio diario es de los más bajos. Viaja más gente y se queda más tiempo; no gasta más cada día. Son dos fenómenos distintos.

---

### Celda 17 — boxplot por región

**Paso rápido.**

**Hablar:**
> Por continente hay diferencias de nivel, pero la región sola no basta: dentro de cada una hay destinos muy dispares.

---

### Celda 18 — matriz de correlación

**Pantalla:** heatmap.

**Hablar:**
> Esta matriz decide el diseño. Gasto total y gasto con tarjeta correlacionan cerca de 0,98: casi la misma información. El gasto medio diario casi no se relaciona con el volumen. Por eso el modelo no recibe ni la tarjeta ni el diario: solo país, mes y año.

---

## 4. Objetivo

**Pantalla:** markdown del objetivo (no hay código).

**Hablar:**
> El target es el gasto turístico mensual en euros. Supervisado y regresión múltiple. Métrica principal: RMSE; también miro R cuadrado y el error absoluto mediano.

---

## 5. Preparación

### Celda 19 — dataset del modelo (quitar nulos del target)

**Hablar:**
> Quito las filas sin gasto publicado. Me quedo con país, mes, año y el target. Unas nueve mil quinientas instancias y ciento treinta y nueve países. No relleno nulos: inventaría la etiqueta.

---

### Celda 20 — one-hot de país y mes

**Hablar:**
> País y mes no tienen orden natural, así que los paso a one-hot: una columna de ceros y unos por cada categoría. El año se queda numérico. Acabamos con unas ciento cincuenta columnas.

---

### Celda 21 — barajar

**Paso rápido.**

**Hablar:**
> Barajo el dataset para que no queden países o meses agrupados al hacer la partición.

---

### Celda 22 — separar `X` e `y`

**Hablar:**
> `y` es el gasto turístico. `X` son solo las características: año y las columnas one-hot de país y mes.

---

### Celda 23 — `train_test_split` 80/20

**Hablar:**
> Ochenta por ciento entrenamiento, veinte por ciento prueba. La prueba no se usa para elegir el modelo ni para afinar hiperparámetros.

---

## 6. Entrenamiento

### Celda 24 — función de validación cruzada

**Paso rápido.**

**Hablar:**
> Defino una validación cruzada con tres particiones. Comparo modelos con RMSE y R cuadrado medios, sin tocar todavía el test.

---

### Celda 25 — regresión lineal (predicciones entrenamiento)

**Pantalla:** alguna fila con error grande o negativo.

**Hablar:**
> Empiezo con regresión lineal. En entrenamiento ya se ven errores grandes y a veces predicciones negativas: la recta suma efectos de país y mes, pero no captura bien las interacciones.

---

### Celda 26 — métricas lineal + CV

**Hablar:**
> En validación cruzada la lineal ronda unos veintiún millones de RMSE y un R cuadrado de unos 0,70. Es la referencia, no el mejor modelo.

---

### Celda 27 — árbol de decisión (predicciones)

**Hablar:**
> El árbol parte con preguntas del tipo “¿es Francia?”, “¿es agosto?”. En entrenamiento acierta casi todo.

---

### Celda 28 — métricas árbol + CV

**Hablar:**
> El RMSE de entrenamiento es cero, pero en validación cruzada sube mucho. Eso es sobreajuste: memoriza los datos vistos. No lo elijo por el error de entrenamiento.

---

### Celda 29 — bosque aleatorio (predicciones)

**Hablar:**
> El bosque promedia muchos árboles. En entrenamiento el error baja, pero no a cero como el árbol solo.

---

### Celda 30 — métricas bosque + CV

**Hablar:**
> En validación cruzada el bosque mejora a la lineal y al árbol: menor RMSE y mayor R cuadrado. De momento es el favorito.

---

### Celda 31 — tabla comparativa

**Pantalla:** tabla ordenada.

**Hablar:**
> Aquí se ven los tres juntos, ordenados por RMSE de validación. Gana el bosque.

---

### Celda 32 — `GridSearchCV` y bosque ajustado

**Hablar:**
> Un hiperparámetro es una opción que fijas *antes* de entrenar, no la aprende el modelo solo: por ejemplo cuántos árboles tiene el bosque o qué profundidad máxima puede tener cada árbol. Como en clase no hemos tocado esto, lo resumo así: en vez de probar a mano, preparo una *rejilla* pequeña: una tabla con unas pocas combinaciones de esos valores. `GridSearchCV` las prueba todas sobre una muestra del entrenamiento, midiendo cada una con la misma validación cruzada. Me quedo con la combinación que mejor sale. Luego el bosque ajustado se vuelve a medir; si mejora respecto al bosque por defecto, se queda; si no, no forzamos el cambio. En este caso sí mejora y queda como mejor modelo.

---

### Celda 33 — elegir `modelo_final`

**Paso rápido.**

**Hablar:**
> El modelo elegido es el bosque ajustado. Lo reentreno con todo el conjunto de entrenamiento.

---

### Celda 34 — evaluación en test + gráfica real vs estimado

**Pantalla:** métricas y el scatter.

**Hablar:**
> Hasta ahora solo hemos comparado modelos con validación cruzada sobre el entrenamiento. El test es el 20 % de filas que apartamos al principio y no hemos usado para elegir ni para ajustar el bosque: es el examen final con datos que el modelo no ha visto. Le pido predicciones de gasto para esas filas y las comparo con el valor real del INE. El RMSE sale de unos diez millones de euros: es la raíz del error cuadrático medio, así que los destinos con mucho gasto pesan más. El error mediano es más bajo, unos cuatrocientos treinta mil, y describe mejor el típico país-mes. El R cuadrado cerca de 0,91 dice que el modelo explica casi todo el patrón del test. En la gráfica, cada punto es un caso real frente a lo estimado; la diagonal roja sería el acierto perfecto. En escala logarítmica se ve que no solo acierta los destinos enormes: también se acerca en los medianos y pequeños.

---

### Celda 35 — prueba temporal 2025–2026

**Hablar:**
> Compruebo también un corte temporal: entreno hasta 2024 y pruebo 2025-2026. El R cuadrado se mantiene alto, cerca de 0,95. El patrón país-mes aguanta de un año a otro.

---

## 7. Conclusiones

**Pantalla:** markdown de conclusiones (léelo resumido, 30–40 s).

**Hablar:**
> En resumen: con país, mes y año se estima el gasto mensual. El bosque gana porque captura destino y temporada. El RMSE hay que leerlo junto al error mediano, porque unos pocos países grandes pesan mucho. Hay límites: faltan datos de pandemia y el modelo no anticipa shocks externos.

---

### Celda 36 — importancia de características

**Hablar:**
> Estas barras muestran qué columnas usa más el bosque. Es orientativo, no causal: señala variables útiles, no causas del gasto.

---

### Celda 37 — predicciones 2027 (Portugal, Japón, Marruecos)

**Hablar:**
> Creo tres escenarios futuros: Portugal en agosto, Japón en febrero y Marruecos en diciembre de 2027. No hay cifra real del INE con la que contrastar: son proyecciones del patrón aprendido.

---

### Celda 38 — histórico del mismo país-mes

**Hablar:**
> Aquí está el histórico del INE para esos mismos meses. Las predicciones de 2027 se parecen al último año visto. Tiene sentido si el destino no cambia de régimen; una huelga o un evento grande no están en las variables.

---

## Cierre (última markdown de entrega)

**Hablar:**
> El cuaderno estará en Colab en modo lectura y este vídeo queda enlazado desde el propio cuaderno. Gracias.

---

## Si vas largo: celdas que puedes saltar casi sin hablar

- Celda 1 (imports)
- Celda 6 (ejemplo Albania)
- Celda 11 (regiones)
- Celda 17 (boxplot región)
- Celda 21 (barajar)
- Celda 24 (definición CV)
- Celda 33 (print del modelo elegido)

## Si vas corto: celdas en las que sí debes pararte

- Celda 2 (datos / fuente)
- Celda 9 o 10 (nulos)
- Celda 16 (agosto: total alto, diario bajo)
- Celda 18 (correlación)
- Celdas 26 / 28 / 30 (métricas de los tres modelos)
- Celda 34 (test)
- Celda 35 (temporal)
- Conclusiones (markdown)

---

## Texto para YouTube

```text
Proyecto completo de Machine Learning — Máster FP IA y Big Data.
Dataset: INE tabla 58692 (gasto turístico por país de destino).
Objetivo: predecir el gasto turístico mensual (euros) con país, mes y año.
Cuaderno Colab: [pegar enlace]
```
