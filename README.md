<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>AUTOMATIZACIÓN DE PROCESOS DE MANUFACTURA</h3>

<h1>Módulo 7 - Controladores industriales</h1>

<h2>Mep Mep Raideres</h2>

<h5>Joan Sebastian Arcila <br>
    Juan Sebastian Daleman Martinez<br>
    Daniel Santiago Muñoz Bernal<br>
    Maria Alejandra Pérez Petro<br>
    Emma Carolina Sarmiento Cabarcas</h5>

<h6>Universidad Nacional de Colombia<br>
    Facultad de Ingeniería<br>
    Departamento de Ingeniería Mecánica y Mecatrónica<br>
    Bogotá, Colombia<br>
    2025</h6>
</div>

<details>
    <summary>📂 Tabla de Contenido</summary>

<!-- TOC -->
- [1. 🤔🎚️ Lógica de Control implementada en Studio 5000](#1-️-lógica-de-control-implementada-en-studio-5000)
  - [1.1. Estructura del Proyecto](#11-estructura-del-proyecto)
  - [1.2. ⚙️🔄️ Lógica en GRAFCET](#12-️️-lógica-en-grafcet)
  - [1.3. 🧠🪜 Lógica en Ladder (LD)](#13--lógica-en-ladder-ld)


</details>

## 1. 🤔🎚️ Lógica de Control implementada en Studio 5000

El sistema de automatización desarrollado tiene como objetivo controlar una **planta de ensamblaje de patinetas eléctricas**, específicamente el movimiento de **7 bandas transportadoras**, cada una con lógica independiente pero estructuralmente idéntica. El control se implementa en un **Controlador CompactLogix 1769-L30ERM**, usando el software **Studio 5000 Logix Designer**, con programación en **GRAFCET (SFC)** y **Ladder Diagram (LD)**.

### 1.1. Estructura del Proyecto

En el controlador `PlantaAutomatizacion`, se define una sola tarea (`MainTask`) que contiene el programa `MainProgram`, el cual tiene dos rutinas:

* `GrafcetBandas`: Contiene la lógica secuencial del movimiento de las bandas mediante diagramas de GRAFCET.
* `MainRoutine`: Lógica complementaria en lenguaje Ladder (LD), que interactúa con señales externas (como Google Assistant) y el variador de frecuencia PowerFlex 525.

### 1.2. ⚙️🔄️ Lógica en GRAFCET

Cada banda cuenta con una estructura típica compuesta por:

* Un paso inicial (`Step_000`).
* Un paso para **activar el motor** (`Step_001`, `Step_004`, etc.).
* Un paso para **detener el motor** (`Step_002`, `Step_005`, etc.).
* Condiciones de transición (`Tran_000`, `Tran_001`, etc.) asociadas a sensores de inicio y parada.
* Acciones (`Action_000`, `Action_001`, etc.) que activan o desactivan las variables tipo BOOL (`Banda1`, `Banda2`, ..., `Banda7`) encargadas de arrancar o detener cada motor.

El GRAFCET está diseñado de manera paralela, lo que permite ejecutar la lógica de todas las bandas simultáneamente sin interferencia entre ellas. Esto se logra mediante el uso de un paso intermedio común (`Step_015`) que ramifica en múltiples caminos, uno para cada banda.

Ejemplo de lógica para una banda (Banda 1):

* **Start**: Si `StartBanda1` está activa y `StopBanda1` no lo está, se transiciona desde `Step_000` a `Step_001`, activando `Banda1`.
* **Stop**: Al cumplirse la condición de `StopBanda1`, se transiciona a `Step_002`, apagando la banda.

### 1.3. 🧠🪜 Lógica en Ladder (LD)

La rutina `MainRoutine` contiene 6 rungs, entre ellos:

1. Activación de la banda 5 mediante Google Assistant (`Google_Str_Banda5`).
2. Desactivación por Google Assistant (`Google_Stp_Banda5`).
3. Envío de comando de frecuencia (`FreqCommand = 6000`) al variador `PowerFlex 525`.
   4-5. Control manual mediante sensores `StartBanda5` y `StopBanda5`.

Estas señales externas están conectadas directamente a las salidas `Start` y `Stop` del variador `Variador_141`, lo que permite controlar físicamente el motor de la banda correspondiente.

Se adjunta a este módulo el archivo PDF del reporte generado desde Studio 5000 Logix Designer, así como el archivo de código fuente del controlador en formato `.ACD`. Estos documentos contienen el detalle completo de la configuración del proyecto, la estructura de tareas, rutinas, variables, lógica en GRAFCET y diagramas en Ladder implementados en el sistema de control industrial.
