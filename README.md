# Gestión de Redes SDN mediante gRPC/gNMI
## Proyecto del Máster en Ingeniería de Telecomunicaciones - Asignatura de Gestión de Redes - EPS/UAM

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)

Este proyecto explora la gestión de Redes Definidas por Software (SDN) utilizando gRPC y gNMI como alternativa moderna a SNMP. Se implementa una interfaz Northbound para la monitorización y configuración de dispositivos de red simulados, demostrando las capacidades de telemetría en tiempo real y gestión centralizada.

**Desarrollado por:**
* [Miguel Carralero Lanchares](https://www.linkedin.com/in/miguel-carralero-lanchares/) <a href="https://www.linkedin.com/in/miguel-carralero-lanchares/" target="_blank"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linkedin/linkedin-original.svg" alt="LinkedIn" width="16" style="vertical-align:middle; margin-left:4px"/></a>
* [Francisco Orcha](https://www.linkedin.com/in/francisco-orcha-38a5831b3/) <a href="https://www.linkedin.com/in/francisco-orcha-38a5831b3/" target="_blank"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linkedin/linkedin-original.svg" alt="LinkedIn" width="16" style="vertical-align:middle; margin-left:4px"/></a>
* [Jaime Marcos](https://www.linkedin.com/in/jaime-marcos-diaz-406100205/) <a href="https://www.linkedin.com/in/jaime-marcos-diaz-406100205/" target="_blank"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linkedin/linkedin-original.svg" alt="LinkedIn" width="16" style="vertical-align:middle; margin-left:4px"/></a>

## Descripción General

El objetivo principal es comparar SNMP y gRPC/gNMI como protocolos de gestión en arquitecturas SDN. Se desarrolla una interfaz Northbound que permite:
*   Configuración dinámica de VLANs.
*   Streaming de métricas de red (CPU, tráfico) en tiempo real.
*   Gestión y reporte de fallos en dispositivos.

El proyecto se divide en dos escenarios principales implementados en Jupyter Notebooks:
1.  **Simulación y Análisis de un Dispositivo Único (`sdn_simulation_analysis.ipynb`):** Se modela un switch con comportamiento de tráfico y CPU realista, incluyendo una fase de fallo programada para observar la telemetría bajo estrés y la detección de anomalías. Se realizan visualizaciones detalladas de las métricas.
2.  **Gestión de Topología Multi-dispositivo (`sdn_topology_management.ipynb`):** Se simula una topología de red jerárquica (core-access-router-firewall). Se implementa la configuración de VLANs en dispositivos específicos, monitoreo concurrente de múltiples dispositivos y un sistema de detección y aislamiento de fallos basado en umbrales de CPU.

## Tecnologías Utilizadas

*   **Lenguaje:** Python 3
*   **Comunicación RPC:** gRPC, Protocol Buffers (Protobuf)
*   **Conceptos SDN/gNMI:** Interfaz Northbound, Telemetría por Streaming, Configuración
*   **Simulación de Red:** Clases personalizadas para modelar dispositivos y topologías
*   **Análisis y Visualización de Datos:** Pandas, NumPy, Matplotlib, Seaborn
*   **Concurrencia:** `threading`, `concurrent.futures` (para el servidor gRPC y cliente en `sdn_topology_management.ipynb`)

## Estructura del Proyecto

```
.
+-- .gitignore
+-- LICENSE
+-- README.md
+-- requirements.txt
+-- docs/
|   +-- Informe_SDN_gRPC_vfinal.pdf    (Informe detallado del proyecto)
+-- images/                            (Capturas de resultados para este README)
|   +-- ...                            (ej: graficos_codigo1.png, output_codigo2.png)
+-- src/
    +-- sdn_northbound.proto           (Definición del servicio gRPC)
    +-- sdn_simulation_analysis.ipynb  (Notebook Código 1: Simulación y análisis de un dispositivo)
    +-- sdn_topology_management.ipynb  (Notebook Código 2: Gestión de topología multi-dispositivo)
    +-- sdn_northbound_pb2.py          (Stubs Python generados por protoc)
    +-- sdn_northbound_pb2_grpc.py     (Stubs Python gRPC generados por protoc)
```

## Instalación y Configuración

Existen dos maneras de configurar el entorno para ejecutar las simulaciones:

**Método 1: Configuración Manual (Recomendado para un entorno limpio)**

1.  **Clonar el repositorio:**
    ```bash
    git clone https://github.com/MiguelCarra/sdn-grpc-network-management.git
    cd sdn-grpc-network-management
    ```

2.  **Crear un entorno virtual (recomendado):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # En Windows: venv\Scripts\activate
    ```

3.  **Instalar dependencias desde `requirements.txt`:**
    Asegúrate de estar en la raíz del repositorio clonado.
    ```bash
    pip install -r requirements.txt
    ```

4.  **Generar Stubs gRPC:**
    Navega a la carpeta `src/` y ejecuta el siguiente comando:
    ```bash
    cd src
    python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. sdn_northbound.proto
    ```
    Esto generará los archivos `sdn_northbound_pb2.py` y `sdn_northbound_pb2_grpc.py` necesarios dentro de la carpeta `src/` (si estos todavía no estan).

**Método 2: Ejecución Directa desde los Jupyter Notebooks (Menos recomendado para producción)**

Si prefieres una ejecución rápida directamente desde los cuadernos de Jupyter (`sdn_simulation_analysis.ipynb` y `sdn_topology_management.ipynb` ubicados en la carpeta `src/`), puedes hacerlo descomentando y ejecutando las primeras celdas de cada notebook.

*   **Paso 1 (Dentro de cada notebook): Instalación de Dependencias**
    Descomenta y ejecuta la celda que contiene:
    ```python
    # !pip install grpcio-tools matplotlib numpy seaborn
    ```
    *Nota: Esto instalará las librerías globalmente si no estás en un entorno virtual, o en el entorno activo del kernel de Jupyter.*

*   **Paso 2 (Dentro de cada notebook): Creación del archivo `.proto`**
    Descomenta y ejecuta la celda que contiene:
    ```python
    # with open('sdn_northbound.proto', 'w') as f:
    #     f.write(""" ...contenido del proto... """)
    ```
    *Nota: Esto creará (o sobrescribirá) el archivo `sdn_northbound.proto` en el mismo directorio donde se está ejecutando el notebook (idealmente `src/`).*

*   **Paso 3 (Dentro de cada notebook): Generación de Stubs gRPC**
    Descomenta y ejecuta la celda que contiene:
    ```python
    # !python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. sdn_northbound.proto
    ```
    *Nota: Esto generará los archivos `_pb2.py` y `_pb2_grpc.py` en el mismo directorio, necesarios para los imports posteriores (si estos todavía no estan).*

Una vez completados estos pasos (ya sea el Método 1 o el Método 2), puedes proceder a ejecutar el resto de las celdas en los notebooks.

## Uso / Ejecución de las Simulaciones

Ambos cuadernos de Jupyter se encuentran en la carpeta `src/`.

### 1. Simulación y Análisis de un Dispositivo Único
   *   Abre y ejecuta el cuaderno `src/sdn_simulation_analysis.ipynb`.
   *   Este notebook:
        *   Define e inicia un servidor gRPC que simula un único switch.
        *   Ejecuta un cliente que solicita métricas (CPU, tráfico) en streaming.
        *   Simula un fallo de CPU al 100% tras 90 segundos.
        *   Genera gráficos detallados de las series temporales de CPU y tráfico, distribuciones (boxplots) y un mapa de calor de la variación de CPU.

### 2. Gestión de Topología Multi-dispositivo
   *   Abre y ejecuta el cuaderno `src/sdn_topology_management.ipynb`.
   *   Este notebook:
        *   Define una topología de red con múltiples dispositivos (core switch, access switch, router, firewall).
        *   Inicia un servidor gRPC que gestiona esta topología.
        *   Un cliente realiza operaciones como:
            *   Configuración de VLANs en switches.
            *   Monitoreo concurrente de métricas de CPU y tráfico para varios dispositivos.
            *   Detección automática de alta CPU en `core_switch` y simulación de aislamiento.
            *   Reporte manual de un fallo en `firewall1`.
        *   La salida se muestra en la consola del notebook, mostrando la telemetría en vivo y los eventos de gestión.

## Resultados y Demostración

Los resultados detallados, análisis comparativo SNMP vs gRPC/gNMI, y conclusiones se encuentran en el **[Informe Completo del Proyecto](docs/Informe_SDN_gRPC.pdf)**.

A continuación, se muestran algunos ejemplos visuales de los resultados obtenidos:

*Ejemplo de Uso de CPU y Tráfico (desde `sdn_simulation_analysis.ipynb`):*
![Captura de pantalla de Figuras 8 y 9 del informe o generada por el notebook](images/codigo1_uso_cpu_trafico.png)


*Ejemplo de Distribuciones de CPU y Tráfico (desde `sdn_simulation_analysis.ipynb`):*
![Captura de pantalla de Figuras 10 y 11 del informe o generada por el notebook](images/codigo1_distribuciones.png)


*Ejemplo de Mapa de Calor de CPU (desde `sdn_simulation_analysis.ipynb`):*
![Captura de pantalla de Figura 12 del informe o generada por el notebook](images/codigo1_heatmap_cpu.png)


*Ejemplo de Salida de Telemetría Multi-dispositivo (desde `sdn_topology_management.ipynb`):*
<pre>🟢 Servidor iniciado - Topología: Core-Access (3 capas)  
🔧 Configuración access_switch1: VLAN 100 agregada a access_switch1. VLANs actuales: [100]  
🔧 Configuración core_switch: VLAN 200 agregada a core_switch. VLANs actuales: [200]  

📊 Métricas en vivo - core_switch:  
⏱️ 17:57:46 | CPU: 83.42% | Tráfico: 359,703 bps  
⏱️ 17:57:47 | CPU: 75.07% | Tráfico: 270,624 bps  
🔴 [Sistema] CPU crítica en core_switch - Acción: Aislar dispositivo  
⏱️ 17:57:49 | CPU: 100.0% | Tráfico: 850,888 bps  
... </pre>  

## Contenido del Informe (docs/Informe_SDN_gRPC_vfinal.pdf)

El informe completo incluye:
*   Introducción y Objetivos
*   Marco Teórico (Arquitectura SDN, Interfaz Northbound, Modelo FCAPS)
*   Análisis Técnico SNMP vs. gRPC/gNMI (Modelo de datos, transporte, seguridad, eficiencia)
*   Implementación Práctica (Desarrollo de la interfaz, sistema de monitorización, visualización)
*   Demostración de Capacidades Únicas de gRPC/gNMI (Telemetría vs Polling, YANG, Escalabilidad)
*   Conclusiones
*   Bibliografía y Anexos

## Posibles Mejoras y Trabajo Futuro

*   Integración completa con modelos YANG para la definición de datos y RPCs.
*   Simulación de escenarios de red más complejos y dinámicos.
*   Desarrollo de una interfaz gráfica de usuario (GUI) para la visualización y gestión.
*   Implementación de mecanismos de seguridad más robustos en gRPC (TLS).
*   Pruebas de rendimiento comparativas más exhaustivas.

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.
