# avionica-reto-2026
## 1. ¿Qué hace el proyecto?

El sistema simula el ciclo completo de adquisición y procesamiento de telemetría de un lanzamiento de cohete muestreado a **10 Hz** (una lectura cada 100 metros) dividido en dos scripts principales:

* **Simulador de Vuelo (`generate_flight.py`):**
  * Modela mediante cinemática básica cinco fases de vuelo: en rampa, propulsión, vuelo balístico bajo gravedad hasta alcanzar un apogeo cercano a los 480 metros, descenso controlado con paracaídas y reposo en tierra.
  * Incorpora comportamiento realista de sensores: ruido gaussiano en el barómetro (0.5 m), disminución de temperatura por gradiente térmico atmosférico estándar y lectura de fuerza específica en el acelerómetro en reposo y en vuelo balistico.
  * Inserta 3 registros corruptos intencionales (campo vacío, texto `ERR` y columna faltante) para simular pérdida de paquetes
  * Genera el archivo crudo `flight_log.csv` con las columnas `time_ms`, `altitude_m`, `accel_z_ms2` y `temp_c`.

* **Procesador de Estación Terrena (`process_flight.py`):**
  * Lee `flight_log.csv` línea por línea, descartando y contabilizando las filas corruptas sin detener la ejecución
  * Filtra la señal de altitud mediante un **promedio móvil** de ventana programado manualmente:
  * Calcula la velocidad vertical por **diferencias finitas centradas** tanto para la altitud cruda como para la filtrada:
  * Detecta **despegue**, **apogeo** y **aterrizaje** con criterios definidos y justificados.
  * Imprime el resumen de las métricas en consola (apogeo y su tiempo, velocidad máxima de ascenso, velocidad promedio de descenso, tiempo total de vuelo y líneas descartadas), guarda `flight_processed.csv` y genera las gráficas de altitud vs. tiempo y velocidad vs. tiempo.

---

## 2. Herramientas y Versiones Usadas

Todos los cálculos, filtros y recorridos se implementaron manualmente sobre listas estándar.

* **Lenguaje:** Python 3.10+ (compatible con Python 3.8+ y Google Colab).
* **Módulos estándar de Python:** `csv`, `math`, `random`.
* **Gráficas:** `matplotlib` (v3.7+ recomendada)

---

## 3. Estructura del Repositorio

```text
├── generate_flight.py    # Simulación física a 10 Hz y generación de flight_log.csv
├── process_flight.py     # Limpieza, filtro de promedio móvil, derivadas, eventos y gráficas
└── README.md             # Documentación completa, pasos de ejecución y notas técnicas

--- 

## 4. Cómo Ejecutar el programa Paso a Paso

Paso 1: Obtener el repositorio
Clona el repositorio en tu equipo o descarga los archivos (`generate_flight.py`, `process_flight.py` y `README.md`) en una misma carpeta:
```bash
git clone [https://github.com/TU-USUARIO/NOMBRE-DEL-REPOSITORIO.git](https://github.com/TU-USUARIO/NOMBRE-DEL-REPOSITORIO.git)
cd NOMBRE-DEL-REPOSITORIO

Gustavo Angel Vazquez Cerritos.
