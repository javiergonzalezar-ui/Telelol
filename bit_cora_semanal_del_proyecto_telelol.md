# Bitácora de Gestión y Avance Técnico: Telelol | LabEnvironment

**Asignatura:** Proyecto de Innovación Tecnológica (Laboratorio Virtual / Laboratorio en el Bolsillo)  
**Docente:** Prof. Ana G. Jorquera F.  
**Temática Asignada:** LabEnvironment — Variables Ambientales (Calor, Fluidos y Calidad del Aire en el Aula)  
**Enlace Interfaz Activa:** [https://javiergonzalezar-ui.github.io/Telelol/](https://javiergonzalezar-ui.github.io/Telelol/)  
**Repositorio Oficial:** [https://github.com/javiergonzalezar-ui/Telelol](https://github.com/javiergonzalezar-ui/Telelol)  

---

## Organización del Equipo y Roles Asignados

| Integrante | Rol Principal | Responsabilidad Primaria |
| :--- | :--- | :--- |
| **Javier González** | Líder del Proyecto (Product Owner) | Coordinación de alcance, cronograma, cumplimiento de Gates, integración End-to-End y despliegue en GitHub Pages. |
| **Eduardo Valenzuela** | Responsable Científico | Definición de fundamentos físicos, variables SI, modelo matemático exponencial y protocolo de validación estadística. |
| **Diego Cariz** | Responsable de Hardware / Instrumentación | Diseño esquemático en Tinkercad, montaje de Arduino Uno con sensores TMP36 y MQ-2, firmware C++ y filtrado de señales. |
| **Gaspar Lineros** | Responsable de Software / Datos | Desarrollo de la Web App, emulador de firmware en JavaScript, integración Web Serial API, gráficos en tiempo real y exportación CSV. |
| **Camilo Gómez** | Responsable Pedagógico / Documentación | Diseño UX/UI, accesibilidad, tutorial interactivo integrado, guía experimental del estudiante y mantención de bitácora. |

---

## Registro Cronológico de Avance (Semanas 1 a 14)

### Etapa 1: Investigación y Definición del Problema (Semanas 1 – 3)

#### Semana 1 — Conformación del Equipo y Exploración de Temática
* **Actividades realizadas:** Asignación de roles internos del grupo **Telelol**, revisión del benchmark de referencia (*Lab4U — Lab in your Pocket*) y análisis de las temáticas propuestas. Se seleccionó la línea **LabEnvironment** enfocada en condiciones ambientales dentro de salas de clases.
* **Acuerdos y decisiones:** Establecer una solución híbrida que permita operar tanto con hardware de bajo costo (Arduino Uno) como mediante simulación web autónoma para eliminar barreras de acceso en colegios y universidades.
* **Evidencia / Participación:** Todo el equipo participa en la definición inicial de alcance y repositorio base.

#### Semana 2 — Levantamiento de Requerimientos y Variable Experimental
* **Actividades realizadas:** Investigación sobre estándares de confort térmico y calidad del aire interior (umbral térmico de $26\text{ }^\circ\text{C}$ y límite de ventilación de $1000\text{ ppm}$ de $\text{CO}_2$). Formulación de la pregunta experimental sobre el impacto de la ocupación y ventilación en el bienestar estudiantil.
* **Responsables clave:** Eduardo Valenzuela (criterios científicos) y Camilo Gómez (definición del usuario objetivo escolar y universitario).

#### Semana 3 — Gate 1: Problema Validado (26 de agosto de 2026)
* **Hito cumplido:** Validación formal de usuario objetivo, necesidad real en el aula, pregunta experimental e instrumentación propuesta.
* **Registro de sesión (Bitácora presencial):** Revisión y consolidación de la ficha inicial del proyecto con asistencia completa:
  * **Javier González:** Participó (consolidación de entregable Gate 1).
  * **Eduardo Valenzuela:** Participó (validación de variables: temperatura, gases/humo y $\text{CO}_2$).
  * **Diego Cariz:** Participó (selección preliminar de sensores TMP36 y MQ-2).
  * **Gaspar Lineros:** Participó (definición de arquitectura web multiplataforma).
  * **Camilo Gómez:** Participó (redacción de ficha de características y necesidad).

---

### Etapa 2: Diseño y Congelamiento de Arquitectura (Semanas 4 – 6)

#### Semana 4 — Modelamiento Físico-Matemático del Recinto
* **Actividades realizadas:** Formulación de las ecuaciones diferenciales de primer orden basadas en la Ley de Enfriamiento/Calentamiento de Newton y balance de masa para gases:
  $$T(t) = T_{ss} + (T_0 - T_{ss})e^{-t/\tau_t}, \qquad C(t) = C_{ss} + (C_0 - C_{ss})e^{-t/\tau_c}$$
* **Parámetros definidos:** Discretización a $\Delta t = 1\text{ s}$ ($1\text{ Hz}$), constantes de tiempo $\tau$ diferenciadas para ventana cerrada ($k = 1$) y ventana abierta ($k = 3$), y ecuaciones de estado estacionario en función de estufa ($H$), humo/gas ($G$) y alumnos ($P$).
* **Responsable:** Eduardo Valenzuela.

#### Semana 5 — Diseño Esquemático de Hardware y Wireframes UX/UI
* **Actividades realizadas:**
  * Construcción del circuito virtual en **Tinkercad** conectando Arduino Uno a sensor de temperatura **TMP36** (pin `A0`) y sensor de gases **MQ-2** (pin `A1`).
  * Diseño de bocetos de interfaz (wireframes) en 3 pantallas: *Inicio*, *Simulador/Captura en tiempo real* (con semáforo expresivo y controles de escenario) y *Resultados*.
* **Responsables:** Diego Cariz (Tinkercad) y Camilo Gómez (Wireframes UX/UI).

#### Semana 6 — Gate 2: Diseño Congelado
* **Hito cumplido:** Entrega del *Informe de Congelamiento de Diseño*, fijando la arquitectura de 3 capas (Entrada $\rightarrow$ Procesamiento $\rightarrow$ Salida), el protocolo de comunicación serial en formato JSON a `9600 baud` y el protocolo de validación estadística ($\bar{X}$, $\sigma$, $E_r\%$).
* **Decisiones congeladas:** Incorporar un tercer sensor virtual de $\text{CO}_2$ en el simulador para enriquecer la experiencia pedagógica sin exigir hardware costoso adicional.

---

### Etapa 3: Desarrollo del Prototipo y Factibilidad Técnica (Semanas 7 – 11)

#### Semana 7 — Gate 3: Prueba de Factibilidad Técnica
* **Hito cumplido:** Verificación funcional de la adquisición de datos y del motor de simulación.
* **Actividades realizadas:**
  * Programación del firmware en C++ para Arduino Uno con lectura ADC de 10 bits, conversión térmica $T(^\circ\text{C}) = (V_{\text{out}} - 0,5) \times 100$ y filtro de media móvil de 5 muestras para suavizar ruido.
  * Validación de la entrega continua de tramas JSON: `{"temperatura", "nivel_gases_contaminantes", "estado_aire", "co2_ppm"}`.
* **Responsables:** Diego Cariz (Firmware C++) y Gaspar Lineros (recepción de datos).

#### Semana 8 — Implementación del Motor de Simulación en JavaScript
* **Actividades realizadas:** Desarrollo del emulador de Arduino en JavaScript (`ES6+`) que replica exactamente el comportamiento del firmware C++ cuando no hay una placa conectada por USB, permitiendo ejecutar el laboratorio desde cualquier navegador o teléfono móvil.
* **Responsable:** Gaspar Lineros.

#### Semana 9 — Desarrollo de Interfaz Interactiva y Visualización Científica
* **Actividades realizadas:** Implementación del esquema visual animado del circuito en la web, el semáforo de alertas con indicadores faciales, los deslizadores de control (*Estufa*, *Humo/Gas*, *Alumnos*) y los tres gráficos en tiempo real con líneas de umbral crítico ($26\text{ }^\circ\text{C}$ y $1000\text{ ppm}$).
* **Responsables:** Gaspar Lineros y Camilo Gómez.

#### Semana 10 — Integración Web Serial API y Terminal de Diagnóstico
* **Actividades realizadas:** Conexión directa entre el navegador (Chrome/Edge) y un Arduino Uno real mediante **Web Serial API** a `9600 baud`, además de la incorporación de una terminal visible en pantalla que muestra las tramas JSON recibidas segundo a segundo.
* **Responsables:** Gaspar Lineros y Javier González.

#### Semana 11 — Módulo de Resultados Estadísticos y Exportación de Datos
* **Actividades realizadas:** Implementación de la pestaña *Resultados*, superponiendo la curva experimental medida frente a la curva teórica ideal, calculando en vivo el promedio ($\bar{X}$), desviación estándar ($\sigma$) y error relativo porcentual ($E_r\%$), junto con la descarga directa en formato **CSV**.
* **Responsables:** Eduardo Valenzuela (fórmulas estadísticas) y Gaspar Lineros (implementación y CSV).

---

### Etapa 4: Integración End-to-End, Documentación y Gate 4 (Semanas 12 – 14)

#### Semana 12 — Tutorial Integrado y Mejoras de Accesibilidad
* **Actividades realizadas:** Incorporación del tutorial interactivo en dos partes dentro de la aplicación (guía de 6 pasos del simulador e instrucciones para editar el circuito en Tinkercad), botones de escenarios rápidos (*Abrir ventana*, *Sala llena*, *Encender un fósforo*), modo oscuro, navegación por teclado y soporte para lectores de pantalla.
* **Responsables:** Camilo Gómez y Gaspar Lineros.

#### Semana 13 — Despliegue en Producción y Pruebas Internas End-to-End
* **Actividades realizadas:** Publicación de la versión integrada en **GitHub Pages** (`https://javiergonzalezar-ui.github.io/Telelol/`), verificación de flujo completo desde la pantalla de bienvenida hasta la exportación CSV, y elaboración de la infografía de guía rápida para usuarios externos.
* **Responsables:** Javier González (despliegue y QA) y Camilo Gómez (infografía de guía).

#### Semana 14 — Gate 4: MVP Integrado (17 de noviembre de 2026)
* **Hito cumplido:** Entrega del **Informe Técnico de Avance (Gate 4)**, demostrando la operación autónoma e integrada de extremo a extremo (*End-to-End*) sin subsistemas aislados.
* **Estado actual del producto:**
  * Interfaz web activa, responsiva y accesible desplegada públicamente.
  * Motor dual operativo (simulación matemática en tiempo real + conexión hardware real por Web Serial).
  * Visualización cuantitativa con unidades SI/UI, semáforo ambiental, análisis estadístico automático y descarga CSV.
  * Propuesta pedagógica preliminar (puntos 1 al 10 de la guía del estudiante) coordinada con los controles reales de la plataforma.

---

## Próximos Pasos (Hacia Gate 5, Gate 6 y Release 1.0)
1. **Validación Experimental Física:** Contrastar lecturas del sensor TMP36 frente a un termómetro digital patrón en al menos 5 repeticiones experimentales y calibrar el MQ-2 mediante la curva $R_s/R_0$ de su hoja de datos.
2. **Pruebas con Usuarios Reales (Gate 6):** Aplicar el laboratorio a una muestra de 5 a 10 estudiantes externos sin intervención del equipo desarrollador, registrando tasa de finalización, tiempo de ejecución y mejoras de interfaz.
3. **Congelamiento Final (Release 1.0):** Completar los puntos 11 al 17 de la guía del estudiante y la guía docente.
