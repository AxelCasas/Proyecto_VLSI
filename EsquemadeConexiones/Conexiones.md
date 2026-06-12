---

### Entregable: Esquema de Conexiones de Hardware (Pinout y Mapeo)

Considerando la tarjeta **Intel DE10-Lite** (basada en el FPGA MAX 10) y los componentes físicos que tienes (ADC, DAC, Amplificador XH-M139 y bocinas de $4\ \Omega$), este documento define el conexionado eléctrico.


```markdown
# 🔌 Guía de Conexiones Físicas y Asignación de Pines (Pinout)

Este documento detalla el mapa de cableado eléctrico entre la tarjeta de desarrollo **Intel DE10-Lite (MAX 10)** y los módulos externos que conforman el ecualizador de audio de 3 bandas.

## 🛠️ Matriz de Asignación de Pines (FPGA a Periféricos)

| Módulo Externo | Pin del Componente | Dirección | Señal en VHDL | Pin Asignado en DE10-Lite |
| :--- | :--- | :--- | :--- | :--- |
| **Reloj Base** | Cristal de Sistema | Entrada | `clk_50MHz` | **PIN_P11** (Reloj interno de 50 MHz) |
| **Control de Usuario** | Switch 0 | Entrada | `reset` | **PIN_C10** (SW0) |
| **Control de Usuario** | Switch 1 / Pot. | Entrada | `gain_g_sel` | **PIN_C11** (SW1 - Control Ganancia Graves) |
| **Control de Usuario** | Switch 2 / Pot. | Entrada | `gain_m_sel` | **PIN_D12** (SW2 - Control Ganancia Medios) |
| **Control de Usuario** | Switch 3 / Pot. | Entrada | `gain_a_sel` | **PIN_C12** (SW3 - Control Ganancia Agudos) |
| **ADC (PCM1808)** | BCK (Bit Clock) | Entrada | `bck_in` | **PIN_W5** (Bloque de Expansión GPIO 0) |
| **ADC (PCM1808)** | LRCK (L/R Clock) | Entrada | `lrck_in` | **PIN_W6** (Bloque de Expansión GPIO 0) |
| **ADC (PCM1808)** | DIN (Serial Data) | Entrada | `din` | **PIN_W7** (Bloque de Expansión GPIO 0) |
| **DAC (PCM5102A)** | BCK (Bit Clock) | Salida | `bck_out` | **PIN_V5** (Bloque de Expansión GPIO 0) |
| **DAC (PCM5102A)** | LRCK (L/R Clock) | Salida | `lrck_out` | **PIN_V7** (Bloque de Expansión GPIO 0) |
| **DAC (PCM5102A)** | DOUT (Serial Data)| Salida | `dout` | **PIN_V8** (Bloque de Expansión GPIO 0) |

## 🔊 Diagrama del Flujo de Conexión de Audio

```mermaid
graph LR
    A[Celular / Fuente Audio] -->|Jack Auxiliar 3.5mm| B(ADC PCM1808)
    B -->|Señales TTL Digitales| C[FPGA DE10-Lite]
    C -->|Audio Procesado I2S| D(DAC PCM5102A)
    D -->|Señal Analógica L/R| E(Amplificador XH-M139 / TPA3116D2)
    E -->|Salida de Potencia| F((Bocinas 4 Ohms / 5W))

    style C fill:#2b3e50,stroke:#333,stroke-width:2px,color:#fff
    style E fill:#1a5276,stroke:#333,stroke-width:1px,color:#fff

