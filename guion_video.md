# Guion del vídeo explicativo — Proyecto ML gasto turístico INE

**Duración objetivo:** 8–9 minutos (máximo 10).  
**Formato:** pantalla compartida del cuaderno `tarea.ipynb` + voz en off o cámara pequeña.  
**Consejo:** habla despacio, no leas el código línea a línea; enseña la celda o la gráfica y explica la idea.

---

## Antes de grabar (checklist)

1. Abre `tarea.ipynb` ya ejecutado (con salidas y gráficas visibles).
2. Ten a mano la tabla de comparación de modelos y el bloque de conclusiones.
3. En YouTube: vídeo público o “no listado”; el enlace irá al final del cuaderno.
4. Tiempo aproximado por bloque (suma ≈ 8,5 min):

| Minuto | Bloque |
| --- | --- |
| 0:00–0:45 | Presentación y problema |
| 0:45–1:45 | Datos (obtención + descripción) |
| 1:45–3:30 | Exploración y gráficas |
| 3:30–4:15 | Objetivo / target |
| 4:15–5:30 | Preparación |
| 5:30–7:30 | Entrenamiento y métricas |
| 7:30–8:45 | Conclusiones y cierre |

---

## 0:00 — Presentación (≈ 45 s)

**En pantalla:** título del cuaderno (primera celda markdown).

**Di:**

> Hola. Soy Jose. Este es mi proyecto completo de Machine Learning.
>
> Trabajo con una estadística del INE: la distribución del gasto turístico de los residentes en España según el país de destino. La tabla es la 58692.
>
> El problema es de regresión supervisada: quiero predecir cuánto se gasta, en euros, en un país concreto durante un mes. Uso esa predicción como si fuera una consultora que necesita anticipar el volumen de gasto hacia cada destino.

---

## 0:45 — Obtención y descripción de los datos (≈ 1 min)

**En pantalla:** celda de carga del CSV y tabla de atributos.

**Di:**

> Los datos no vienen de un sensor ni de una encuesta propia. Son una estadística oficial del Instituto Nacional de Estadística. El fichero es un CSV separado por punto y coma.
>
> En bruto hay cuatro columnas: el país de destino, el tipo de magnitud, el periodo mensual —por ejemplo 2024M08— y el importe en texto. Hay tres magnitudes: gasto turístico total, gasto medio diario y gasto con tarjeta. Todo está en euros.
>
> El fichero viene en formato largo: una fila por país, magnitud y mes. En total unas cuarenta y cuatro mil filas, con ciento sesenta y cinco países y noventa meses, de enero de 2019 a junio de 2026.
>
> Cuando el INE no publica la cifra, aparece un punto o una celda vacía. Eso no es un cero: es un dato no publicado.

---

## 1:45 — Exploración y visualización (≈ 1 min 45 s)

**En pantalla:** histogramas, serie temporal, correlación. Ve pasando las gráficas mientras hablas.

**Di:**

> Primero limpio los importes: el INE usa punto de miles y coma decimal. Después paso a formato ancho: una fila por país y mes, con una columna para cada magnitud.
>
> Aquí se ve el sesgo del gasto total: casi todos los países-mes son relativamente bajos y unos pocos destinos grandes estiran la escala. Por eso el histograma del total lo muestro hasta veinte millones de euros, y la cola grande se entiende mejor en escala logarítmica.
>
> También hay estacionalidad clara: en verano el gasto total sube. En 2020 y 2021 faltan muchos datos —alrededor del 64 % en 2020 y la mitad en 2021— por las restricciones de viaje.
>
> Un detalle importante: el gasto total y el gasto con tarjeta tienen una correlación cercana a 0,98. Se mueven juntos y el INE las publica a la vez. Si metiera el gasto con tarjeta como predictor, casi le estaría dando al modelo la respuesta.
>
> El gasto medio diario, en cambio, cuenta otra historia: en agosto el volumen sube, pero el gasto por día no. Por eso no lo uso como entrada del modelo.

---

## 3:30 — Objetivo del proyecto (≈ 45 s)

**En pantalla:** apartado “Exposición del objetivo”.

**Di:**

> El target es el gasto turístico mensual: euros gastados por los residentes en España en un país de destino durante un mes.
>
> Es aprendizaje supervisado porque el INE ya etiquetó cada país-mes. Es regresión múltiple porque predigo un número a partir de varias características.
>
> La métrica principal es el RMSE en euros. También miro el R cuadrado y el error absoluto mediano, porque el RMSE se deja arrastrar por destinos enormes como Francia o Portugal.

---

## 4:15 — Preparación de los datos (≈ 1 min 15 s)

**En pantalla:** preparación, one-hot, `train_test_split`.

**Di:**

> Para entrenar solo uso tres predictores: el país, el mes y el año. No uso la región, ni el gasto diario, ni el gasto con tarjeta.
>
> Quito las filas sin gasto publicado. No relleno con la mediana: inventaría una etiqueta. Me quedo con unas nueve mil quinientas instancias y ciento treinta y nueve países.
>
> El país y el mes son categorías: los paso a one-hot, columnas de ceros y unos. El año se queda como número.
>
> Barajo los datos y hago una partición 80-20: entrenamiento y prueba. El conjunto de prueba no entra en el ajuste de hiperparámetros. Más adelante repito la evaluación con un corte temporal: entrenar hasta 2024 y probar 2025-2026.

---

## 5:30 — Entrenamiento y rendimiento (≈ 2 min)

**En pantalla:** comparación de modelos, GridSearch, métricas de test y prueba temporal.

**Di:**

> Comparo tres modelos: regresión lineal, árbol de decisión y bosque aleatorio. Los evalúo primero con validación cruzada sobre el entrenamiento.
>
> La regresión lineal es la referencia. Solo suma efecto de país, efecto de mes y tendencia anual. No captura bien las interacciones y a veces estima gastos negativos.
>
> El árbol memoriza el entrenamiento: el error ahí llega a cero, pero en validación cruzada empeora. Eso es sobreajuste.
>
> El bosque aleatorio equilibra mejor. Tras una búsqueda pequeña de hiperparámetros, el modelo elegido es el bosque ajustado.
>
> En el conjunto de prueba, el RMSE ronda los diez millones de euros, el error absoluto mediano unos cuatrocientos treinta mil euros, y el R cuadrado alrededor de 0,91.
>
> En la prueba temporal, entrenando solo hasta 2024 y evaluando 2025-2026, el R cuadrado se mantiene alto, cerca de 0,95. El patrón país-mes es bastante estable de un año a otro.
>
> Interpreto el RMSE con cuidado: la mediana del gasto mensual está cerca de un millón y medio de euros, y unos pocos destinos superan los cien millones. El error mediano describe mejor el caso típico.

---

## 7:30 — Conclusiones y cierre (≈ 1 min 15 s)

**En pantalla:** conclusiones + predicciones de ejemplo (Portugal, Japón, Marruecos) si quieres.

**Di:**

> En resumen: con país, mes y año se puede estimar el gasto turístico mensual del INE. El bosque supera a la recta porque captura la interacción entre destino y temporada.
>
> El proyecto tiene límites claros. Faltan muchos países-mes de 2020 y 2021, y varios países no tienen ninguna cifra. El modelo aproxima la estadística oficial; no explica por qué crece un destino ni anticipa shocks como una huelga aérea.
>
> Las predicciones de 2027 son escenarios: prolongan el patrón visto, no son un dato del INE.
>
> El cuaderno está en Google Colab en modo lectura y este vídeo queda enlazado desde el propio cuaderno.
>
> Gracias por verlo.

---

## Frases de apoyo si te sobra o falta tiempo

**Si vas corto (añade 20–30 s):**

- Enseña una gráfica de los cinco destinos grandes y di: “Francia, Italia o Portugal repiten el mismo dibujo cada año; eso es lo que el modelo puede aprender.”
- Enseña la importancia de características del bosque y di: “Estas barras son orientativas: dicen qué columnas usa el modelo, no una causa.”

**Si vas largo (recorta):**

- Omite la fórmula del RMSE; basta decir “penaliza más los errores grandes y se mide en euros”.
- Omite el detalle de GridSearch; di solo “ajusté una rejilla pequeña y me quedé con el mejor RMSE de validación”.
- Omite los tres escenarios de 2027.

---

## Texto corto para la descripción de YouTube

```text
Proyecto completo de Machine Learning — Máster FP IA y Big Data.
Dataset: INE tabla 58692 (gasto turístico por país de destino).
Objetivo: predecir el gasto turístico mensual (euros) a partir de país, mes y año.
Cuaderno Colab: [pegar enlace]
```

---

## Tras grabar

1. Sube el vídeo a YouTube.
2. Pega el enlace en la celda final del cuaderno (`tarea.ipynb`), junto al enlace de Colab.
3. Comprueba que el vídeo se abre sin login (público o no listado).
