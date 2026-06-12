# 🤖 Especificación de la Máquina de Estados (FSM) - Módulo I2S Receiver

Este módulo es el encargado de sincronizarse con los relojes de audio del ADC externos (`BCK` y `LRCK`) para capturar el flujo de datos seriales enlazado al pin `DIN`, reconstruyendo las muestras de audio paralelas de 24 bits para los canales izquierdo y derecho.

## Diagrama de Transición de Estados (Mermaid)

```mermaid
stateDiagram-v2
    [*] --> ST_IDLE : reset = '1'
    
    ST_IDLE --> ST_WAIT_LRCK : reset = '0'
    ST_WAIT_LRCK --> ST_READ_LEFT : Detecta flanco (Cambio de canal L/R)
    
    ST_READ_LEFT --> ST_READ_RIGHT : Bit_Counter = 24 (Lectura L terminada)
    ST_READ_LEFT --> ST_READ_LEFT : BCK flanco activo / Bit_Counter < 24
    
    ST_READ_RIGHT --> ST_WAIT_LRCK : Bit_Counter = 24 / Activa data_ready
    ST_READ_RIGHT --> ST_READ_RIGHT : BCK flanco activo / Bit_Counter < 24
