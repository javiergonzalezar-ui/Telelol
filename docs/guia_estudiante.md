# Telelol | LabEnvironment
## Guía del estudiante: calidad del aire y confort térmico en una sala de clases

**Versión:** 0.1 — guía didáctica para el MVP de Semana 14 (17 de noviembre de 2026)  
**Modalidad:** laboratorio virtual con datos simulados  
**Aplicación:** https://javiergonzalezar-ui.github.io/Telelol/  
**Repositorio:** https://github.com/javiergonzalezar-ui/Telelol  
**Equipo del proyecto:** Javier González (coordinación), Eduardo Valenzuela (fundamento científico), Diego Cariz (hardware/instrumentación), Gaspar Lineros (software/datos) y Camilo Gómez (pedagogía/documentación).

> **Antes de comenzar:** Telelol permite explorar un modelo simplificado de condiciones ambientales. El CO₂ de la web es simulado; el indicador de gases/humo usa una conversión orientativa de una lectura tipo MQ-2 y **no constituye una medición calibrada en ppm**. El laboratorio sirve para aprender, no para certificar la seguridad del aire de una sala real.

## 1. Nombre del laboratorio y versión

**Telelol — LabEnvironment: El vigilante del aire de tu sala de clases.** Guía del estudiante, versión 0.1, correspondiente al prototipo web disponible en la Semana 14. Esta versión describe las funcionalidades observadas en el archivo `index.html`; no se presenta como una versión final validada.

## 2. Público objetivo y conocimientos previos

Dirigida a **estudiantes universitarios iniciales, particularmente de segundo año de Ingeniería Civil**, en actividades introductorias de termodinámica, transferencia de calor y variables ambientales. Conviene saber leer gráficos de líneas y reconocer temperatura, tiempo, concentración, unidades y promedios. No se requieren conocimientos de programación ni disponer de Arduino físico para la actividad principal.

## 3. Duración estimada

**45–60 minutos** en una sesión planificada: 10 minutos para explorar la aplicación, 20–25 minutos para comparar condiciones de ventilación, 10–15 minutos para registrar y analizar datos, y 5–10 minutos para comunicar conclusiones. Estos tiempos son orientativos, no resultados de una aplicación medida en aula.

## 4. Objetivo general y resultados de aprendizaje

**Objetivo general:** analizar cómo la ocupación, una fuente de calor y la ventilación modifican las tendencias de temperatura y CO₂ simulados en una sala de clases.

Al finalizar, el estudiante podrá:

- Identificar las variables de entrada (estufa/calor, humo o gas, alumnos y ventana) y las variables de salida (temperatura, indicador de gases y CO₂ simulado).
- Comparar dos escenarios cambiando **solo la ventilación** y manteniendo iguales las demás condiciones.
- Interpretar la dirección y la velocidad de cambio de las curvas temporales.
- Descargar un CSV y utilizarlo como evidencia cuantitativa.
- Distinguir datos simulados, lectura analógica aproximada y medición física calibrada.

## 5. Fundamento científico, ecuaciones y conceptos

En un modelo de primer orden, una magnitud ambiental `x` se aproxima gradualmente a un valor de equilibrio `x_ss`. El simulador actualiza las variables en pasos nominales de un segundo:

`x[n+1] = x[n] + α · k · (x_ss − x[n])`

- `x[n]`: valor simulado en el instante `n`.
- `α`: coeficiente de respuesta de la variable.
- `k`: factor asociado a la ventilación (`1` con la ventana cerrada y `3` con la ventana abierta).
- `x_ss`: valor de equilibrio correspondiente a las condiciones elegidas.

Como referencia continua se utiliza la forma exponencial:

`x(t) = x_ss + (x_0 − x_ss) · exp(−t/τ)`

En la simulación, las condiciones de equilibrio con la **ventana cerrada** dependen de:

- **Temperatura:** `T_ss = 21 + 0,2·H + 0,1·P` (°C).
- **Gases/humo del modelo:** `G_ss = 30·G` (escala interna del simulador).
- **CO₂:** `CO2_ss = 420 + 55·P` (ppm simuladas).

Aquí `H` es el control de estufa (0–100 %), `G` el de humo/gas (0–100 %) y `P` la cantidad de alumnos (0–40). Al abrir la ventana, el algoritmo también reduce los equilibrios, por lo que el sistema no solo responde más rápido: tiende a niveles inferiores.

La interfaz presenta temperatura en **°C**, con conversión `T(K) = T(°C) + 273,15`; presenta CO₂ simulado en **ppm**. Para gases/humo, la etiqueta «ppm» de la web es **aproximada** y no debe interpretarse como concentración física certificada de un gas específico.

El sistema muestra referencias visuales de **26 °C** (temperatura) y **1000 ppm** (CO₂ simulado). Para gases/humo, el firmware utiliza una **lectura analógica filtrada superior a 550** como criterio de alerta; 550 **no es un umbral calibrado en ppm**.

## 6. Materiales y dispositivos requeridos

**Obligatorios:** computador o teléfono con navegador actualizado, conexión a internet, acceso a Telelol y una aplicación para abrir CSV (Excel, LibreOffice Calc o similar) si se hará análisis tabular.

**Opcionales, para una extensión con hardware:** Arduino Uno, sensor TMP36, sensor MQ-2, protoboard, cables, cable USB y computador con Chrome o Edge y soporte Web Serial. La práctica principal **no necesita conectar estos elementos**.

## 7. Preparación e instalación

1. Abre https://javiergonzalezar-ui.github.io/Telelol/.
2. Haz clic en **«Comenzar simulación»**.
3. Si es tu primera visita, entra en **«Tutorial»**, revisa «Cómo usar el simulador» y vuelve a **«Simulador»**.
4. En **«Tú controlas la sala»**, selecciona **estufa 0 %**, **humo/gas 0 %** y **alumnos 0**. También puedes presionar **«Dejar todo limpio»**.
5. Verifica que el botón de ventilación indique **«Abrir ventana»**: significa que la ventana está cerrada.
6. Mantén **«Velocidad x1»** para que cada actualización represente aproximadamente un segundo nominal. No uses x2/x4 si necesitas comparar tiempos del CSV como segundos.
7. No es necesario instalar software ni conectar Arduino para usar la simulación web.

## 8. Pregunta de investigación

**¿Cómo influye abrir una ventana en la rapidez y los niveles finales de temperatura y CO₂ simulados de una sala ocupada, manteniendo igual el número de alumnos y la potencia de la estufa?**

## 9. Hipótesis o predicción inicial

**Si se abre la ventana y se mantienen constantes la cantidad de alumnos y la potencia de la estufa, entonces la temperatura y el CO₂ simulados tenderán más rápidamente a valores de equilibrio menores que con la ventana cerrada.**

Antes del experimento, registra si estás de acuerdo y explica en una oración por qué.

## 10. Procedimiento paso a paso

### Ensayo A: ventana cerrada

1. Entra en **«Simulador»** y presiona **«Dejar todo limpio»**. Si el botón dice «Cerrar ventana», presiónalo para dejar la ventana cerrada.
2. Configura **estufa 85 %**, **humo/gas 0 %** y **35 alumnos**. No cambies esos controles durante el ensayo.
3. Pulsa **«Encender»**. Comienza un registro nuevo con las condiciones iniciales del simulador.
4. Espera cinco actualizaciones iniciales: el promedio móvil arranca con muestras en cero y puede mostrar lecturas transitorias artificialmente bajas.
5. Observa el semáforo, las tarjetas numéricas, las curvas de temperatura y CO₂ y la sección **«Lo que escribe el Arduino»**.
6. Deja transcurrir **al menos 30 actualizaciones en x1**. Anota los valores de temperatura y CO₂ aproximadamente en las actualizaciones 10, 20 y 30.
7. Pulsa **«Descargar CSV»**. Renombra el archivo descargado como `ensayo_A_ventana_cerrada.csv` y guárdalo. Presiona **«Apagar»**.

### Ensayo B: ventana abierta

8. Con la aplicación apagada, presiona **«Dejar todo limpio»** y vuelve a fijar **estufa 85 %**, **humo/gas 0 %** y **35 alumnos**.
9. Pulsa **«Abrir ventana»**. El botón debe cambiar a **«Cerrar ventana»**, indicando ventana abierta.
10. Presiona **«Encender»**; se reinicia el registro. Espera nuevamente cinco actualizaciones antes de interpretar lecturas filtradas.
11. Observa otros **30 pasos en x1** sin modificar los controles. Anota temperatura y CO₂ en los mismos instantes aproximados que en el Ensayo A.
12. Presiona **«Descargar CSV»** y guarda el archivo como `ensayo_B_ventana_abierta.csv`. Después, presiona **«Apagar»**.

### Comparación y cierre

13. Abre ambos CSV en tu planilla. Utiliza las columnas `segundo`, `temperatura_C`, `gases_ppm`, `co2_ppm` y `estado_aire`.
14. Compara las columnas de temperatura y CO₂ en los mismos números de actualización. **No atribuyas estas diferencias a mediciones reales de una sala**: son resultados del modelo computacional.
15. Escribe una conclusión basada en los datos: ¿qué cambió con la ventilación?, ¿en cuál ensayo la curva se aproximó más rápido a un nivel estable?, ¿se apoyó la hipótesis?
16. Como actividad opcional de repetibilidad, repite cada escenario con idénticos parámetros y compara los CSV. La planificación de validación del grupo contempla **cinco repeticiones por condición**, pero esta guía no afirma que esas pruebas ya se hayan realizado.

## 11. Tabla o mecanismo de registro de datos

Registra las condiciones de cada ensayo y transcribe los valores de los CSV. **No completes la tabla con resultados inventados.**

| Ensayo | Ventana | Estufa (%) | Humo/gas (%) | Alumnos | Actualización | Temperatura (°C) | CO₂ simulado (ppm) | Semáforo |
|---|---|---:|---:|---:|---:|---:|---:|---|
| A | Cerrada | 85 | 0 | 35 | 10 | ____ | ____ | ____ |
| A | Cerrada | 85 | 0 | 35 | 20 | ____ | ____ | ____ |
| A | Cerrada | 85 | 0 | 35 | 30 | ____ | ____ | ____ |
| B | Abierta | 85 | 0 | 35 | 10 | ____ | ____ | ____ |
| B | Abierta | 85 | 0 | 35 | 20 | ____ | ____ | ____ |
| B | Abierta | 85 | 0 | 35 | 30 | ____ | ____ | ____ |

Si exportas a velocidad x2/x4, la columna «segundo» sigue contando pasos de simulación y **no debe confundirse con segundos reales de reloj**.

## 12. Gráficos y cálculos requeridos

1. Elabora un gráfico de **temperatura (°C) frente a actualización/tiempo nominal** para A y B en los mismos ejes.
2. Elabora otro de **CO₂ simulado (ppm) frente a actualización/tiempo nominal** para A y B.
3. Compara las diferencias `ΔT = T_B − T_A` y `ΔCO₂ = CO₂_B − CO₂_A` en la actualización 30.
4. Si realizaste repeticiones, calcula la media de cada magnitud para el instante elegido: `promedio = suma de observaciones / número de observaciones`.

El CSV descargado **no incorpora por sí solo** un análisis automatizado de incertidumbre ni la curva teórica superpuesta; esos cálculos, si se solicitan, se realizan externamente.

## 13. Preguntas de análisis

1. ¿Qué parámetro cambió entre A y B? ¿Cuáles permanecieron constantes?
2. ¿Qué ocurrió con las curvas cuando se abrió la ventana?
3. ¿Por qué una respuesta de primer orden no cambia instantáneamente de un equilibrio a otro?
4. ¿Qué diferencias encuentras entre un indicador de gases no calibrado y una medición física en ppm?
5. ¿El semáforo coincide con lo que muestran los gráficos y sus límites de referencia?
6. ¿Qué cambiaría si aumentas a 40 alumnos o reduces la potencia de la estufa?

## 14. Discusión de error e incertidumbre

La simulación contiene variaciones aleatorias pequeñas y una media móvil de cinco lecturas. Además, los primeros datos se ven afectados porque el historial parte con ceros. Por eso, explica cómo pueden cambiar levemente dos ejecuciones con los mismos controles.

**Limitaciones a declarar:** el CO₂ se calcula en la web y no proviene de un sensor físico; la escala de «gases ppm» es aproximada; no hay una calibración certificada del MQ-2; no se presentan resultados de ensayos físicos reales. Si se compara con un valor teórico externo, identifica la fuente y usa una definición de error apropiada antes de calcular porcentajes.

## 15. Conclusiones y conexión con aplicaciones reales

Redacta un párrafo corto con **una observación cuantitativa de A**, **una observación cuantitativa de B**, tu interpretación y la decisión sobre la hipótesis. Relaciona el ejercicio con ventilación de salas, confort térmico o monitoreo ambiental, aclarando que el prototipo no reemplaza instrumentos profesionales.

**Plantilla para completar:** «En el paso 30, el ensayo con ventana cerrada registró ____ °C y ____ ppm de CO₂ simulado. Con ventana abierta registró ____ °C y ____ ppm. Esta diferencia indica que ____. Por ello, la hipótesis se [apoya/no se apoya] dentro del modelo utilizado».

## 16. Recomendaciones de seguridad y uso

- El experimento principal es **virtual**: no se requiere producir humo, liberar gases, encender fósforos ni calentar una sala real.
- No utilices las alertas educativas del prototipo para decidir si un recinto real es seguro.
- Si se trabaja con hardware, utiliza conexiones de baja tensión y sigue las indicaciones del docente. No manipules gases combustibles ni montajes de combustión para probar el MQ-2.
- El modo **«Conectar Arduino real»** es opcional, requiere equipo compatible y no forma parte de la validación documentada con el material disponible.

## 17. Solución orientativa para el docente

**Resultado cualitativo esperado en la simulación:** con `H = 85`, `G = 0` y `P = 35`, el escenario de ventana cerrada presenta una tendencia de temperatura y CO₂ hacia niveles más altos que con ventana abierta. La apertura aumenta la velocidad de aproximación y disminuye el equilibrio previsto por el algoritmo. Las cifras concretas deben obtenerse ejecutando y exportando los ensayos; esta guía no presenta mediciones de una validación realizada.

**Criterios de revisión sugeridos:** comparación válida con iguales parámetros, dos CSV identificados, dos gráficos rotulados, interpretación de los límites, reconocimiento de datos simulados y conclusión respaldada con valores. La tabla puede completarse por cada estudiante durante la actividad.
