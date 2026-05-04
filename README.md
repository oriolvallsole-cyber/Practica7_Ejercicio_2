🎤🔊 Grabación y Reproducción de Audio con I2S en ESP32 (INMP441 + MAX98357A)

Proyecto que implementa un sistema completo de captura y reproducción de audio digital usando un ESP32, combinando:

🎤 Micrófono digital I2S → INMP441
🔊 Amplificador DAC I2S → MAX98357A
📌 Descripción

Este proyecto permite:

Grabar audio desde un micrófono I2S (INMP441)
Procesar las muestras en tiempo real
Almacenar temporalmente en memoria
Reproducir el audio grabado mediante un amplificador I2S

Se utilizan dos buses I2S independientes del ESP32:

Uno en modo RX (entrada) → micrófono
Otro en modo TX (salida) → altavoz
🧠 Conceptos clave
I2S Full-Duplex (simulado): uso de dos periféricos I2S
Muestreo de audio: 16 kHz
Conversión de 32 → 16 bits
Procesamiento de señal (RMS, pico)
Buffers DMA para audio en tiempo real
🔌 Hardware requerido
ESP32
Micrófono I2S (INMP441)
Amplificador I2S (MAX98357A)
Altavoz
Cables
🔧 Conexiones
🔊 Altavoz (MAX98357A)
ESP32 GPIO	Módulo
GPIO 4	BCLK
GPIO 5	LRC
GPIO 6	DIN
🎤 Micrófono (INMP441)
ESP32 GPIO	Módulo
GPIO 7	BCLK
GPIO 15	WS
GPIO 16	DOUT
GND	L/R
MIC (INMP441)        ESP32        MAX98357A (Speaker)
--------------       ------       -------------------
SCK (BCLK)   ---->   GPIO7
WS           ---->   GPIO15
SD (DOUT)    ---->   GPIO16

                             GPIO4  ----> BCLK
                             GPIO5  ----> LRC
                             GPIO6  ----> DIN
⚙️ Configuración del sistema
Frecuencia de muestreo: 16,000 Hz
Duración de grabación: 2 segundos
Formato entrada: 32 bits
Formato salida: 16 bits
Canal: mono (duplicado a estéreo)
🚀 Funcionamiento
1. Inicialización (setup())
Configuración de ambos buses I2S:
RX → micrófono
TX → altavoz
Visualización de pines por Serial
Cuenta atrás para grabación
2. Grabación (recordFromMic())
Captura datos en bloques (DMA)
Conversión:
32 bits → 16 bits (shift)
Cálculo en tiempo real:
Pico
Energía
RMS

Ejemplo de salida:

Grabando...  45.3% | pico:  1023 | rms:   345
3. Reproducción (playRecordingOnce())
Reproduce el buffer grabado
Convierte mono → estéreo duplicando muestras
Envío por I2S al amplificador
4. Loop infinito
Reproduce continuamente la grabación
Añade pequeña pausa entre repeticiones
🧾 Código principal
// (código completo aquí, omitido en README real si es muy largo)
// 👉 Recomendación: mantenerlo en /src/main.cpp
📊 Procesamiento de señal

Durante la grabación se calculan:

Pico (Peak): amplitud máxima
RMS (Root Mean Square): nivel medio de señal

Esto permite evaluar la calidad del audio en tiempo real.

⚠️ Consideraciones importantes
El micrófono entrega datos en 24 bits (alineados a 32 bits)
Se aplica un bit shift (14 bits) para adaptar la señal
Se utiliza clamping para evitar overflow en 16 bits
El audio se almacena completamente en RAM
📈 Ventajas
✔ Sistema completo de entrada/salida de audio
✔ Procesamiento en tiempo real
✔ Uso avanzado de I2S en ESP32
✔ Sin necesidad de almacenamiento externo
⚠️ Limitaciones
Memoria RAM limitada (grabación corta)
No hay almacenamiento persistente
Calidad dependiente del micrófono y configuración
🔮 Posibles mejoras
Guardar audio en tarjeta SD
Streaming en tiempo real (WiFi/Bluetooth)
Filtros digitales (FFT, EQ)
Compresión (MP3/AAC)
Grabación continua tipo buffer circular
📚 Referencias
Documentación práctica I2S
Datasheet INMP441
Datasheet MAX98357A
