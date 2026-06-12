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
    A[Celular / Fuente Audio] --> B(ADC PCM1808)
    B --> C[FPGA DE10-Lite]
    C --> D(DAC PCM5102A)
    D --> E(Amplificador TPA3116d2)
    E --> F((Bocinas 4 Ohms / 5W))
