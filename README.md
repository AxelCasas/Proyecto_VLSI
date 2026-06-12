# Procesador Digital de Audio: Ecualizador de 3 Bandas en FPGA

### Diseño Digital VLSI - Proyecto Final 

## 🎯 Objetivo
Diseñar e implementar un sistema de procesamiento digital de audio en FPGA capaz de adquirir una señal de audio, aplicar filtrado digital en tres bandas (graves, medios y agudos), y reproducir la señal procesada en una bocina

## 📋 Especificaciones del Sistema

* **Adquisición de audio:** Uso de un ADC de audio (PCM1808 o equivalente) desde fuentes analógicas (Bluetooth, jack, etc.)
* **Interfaz digital:** Captura de datos mediante interfaz serial de audio I2S (Sincronización con BCK, LRCK y reconstrucción de muestras mono/estéreo)
* **Procesamiento digital:** Ecualizador de 3 bandas mediante filtros digitales (FIR recomendado) con control de ganancia independiente
  * **Graves:** 20 Hz - 300 Hz
  * **Medios:** 300 Hz - 4 kHz
  * **Agudos:** 4 kHz - 20 kHz
* **Salida de audio:** Uso de un DAC de audio (PCM5102A o equivalente) o alternativa (PWM + filtro RC / Red R-2R)
* **Amplificación:** Integración con un amplificador de audio (familia TPA o equivalente) para reproducción en bocina
* **Interfaz de usuario:** Control de ganancia por banda mediante switches o botones

---

## 🛠️ Arquitectura General del Sistema (Hardware)

```mermaid
graph LR
    subgraph Entrada["Fuentes Analógicas"]
        A[Bluetooth / Jack / Celular]
    end

    subgraph Adquisicion["Módulo de Captura"]
        B(ADC: PCM1808)
    end

    subgraph Procesamiento["Dispositivo Central"]
        C[[FPGA]]
    end

    subgraph Salida["Conversión y Amplificación"]
        D(DAC: PCM5102A / PWM)
        E[Amplificador: TPA3116d2]
        F((Bocina))
    end

    A -->|Señal Analógica| B
    B -->|Protocolo I2S: BCK, LRCK, DIN| C
    C -->|Protocolo I2S: BCK, LRCK, DOUT| D
    D -->|Audio Filtrado| E
    E -->|Audio Amplificado| F
```

---

---

## 💻 Arquitectura Interna de la FPGA

```mermaid
graph TD
    BCK_IN[Pin: BCK_IN] -->|Reloj de Bits| I2S_RX(Módulo de Captura: I2S Receiver)
    LRCK_IN[Pin: LRCK_IN] -->|Reloj L/R| I2S_RX
    DIN[Pin: DIN] -->|Datos Seriales| I2S_RX
    
    CTRL_IN[Switches / Botones] --> UI_CTRL[Módulo de Control de Ganancia]

    subgraph FPGA [" Arquitectura Interna (Procesamiento en Tiempo Real) "]
        I2S_RX -->|Muestra Paralelo| BUFF[Buffer de Muestras]
        
        BUFF --> FIR_G(Filtro FIR: Graves<br>20 Hz - 300 Hz)
        BUFF --> FIR_M(Filtro FIR: Medios<br>300 Hz - 4 kHz)
        BUFF --> FIR_A(Filtro FIR: Agudos<br>4 kHz - 20 kHz)
        
        UI_CTRL -->|G_g| GAIN_G[Multiplicador Graves]
        UI_CTRL -->|G_m| GAIN_M[Multiplicador Medios]
        UI_CTRL -->|G_a| GAIN_A[Multiplicador Agudos]
        
        FIR_G --> GAIN_G
        FIR_M --> GAIN_M
        FIR_A --> GAIN_A
        
        GAIN_G --> MIXER{Mezclador / Sumador<br>con Saturación}
        GAIN_M --> MIXER
        GAIN_A --> MIXER
        
        MIXER -->|Muestra Mezclada| I2S_TX(Módulo de Salida: I2S Transmitter)
    end

    I2S_TX -->|Datos Seriales| DOUT[Pin: DOUT]
    I2S_TX --> BCK_OUT[Pin: BCK_OUT]
    I2S_TX --> LRCK_OUT[Pin: LRCK_OUT]
```

