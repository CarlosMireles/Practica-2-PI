# Práctica Lab2: Interfaces Paralelos
Esta práctica involucra la implementación de un programa para el manejo de pulsadores, un teclado matricial y un display de 7 segmentos en una placa de desarrollo, utilizando diferentes modos de visualización y un frecuencímetro. A continuación, se detallan los aspectos clave de la práctica:

### Descripción de la práctica

1. **Configuración de Pines**:
   - Los pulsadores están conectados al puerto `PORTC` con las siguientes asignaciones: `PRIGHT`, `PDOWN`, `PLEFT`, `PENTER`, `PUP` y `SPEAKER`.
   - El teclado matricial y el display de 7 segmentos comparten el puerto `PORTL`, en el cual las líneas de filas del teclado (`R1`, `R2`, `R3`, `R4`) están asignadas a entradas, mientras que las salidas (`D1`, `D2`, `D3`, `D4`) controlan los cátodos del display.

2. **Inicialización del Sistema**:
   - Se utiliza `Serial.begin(9600)` para establecer la comunicación serial.
   - `DDRA` se configura para controlar los segmentos del display y el `PORTA` se inicializa para mostrar el número `0`.
   - Se programa el temporizador `TIMER3` en modo CTC para la generación de interrupciones periódicas que permitan el manejo de los displays y el teclado de forma intercalada (multiplexación).

3. **Modos de Operación**:
   - Modo 1: Visualización normal con tres dígitos.
   - Modo 2: Visualización reducida inferior (solo decenas y unidades).
   - Modo 3: Visualización reducida superior (solo centenas y decenas).
   - Modo 4: Frecuencímetro, donde se calcula la frecuencia de una señal de entrada en el pin 5 de la placa.

4. **Funciones de Manejo de Pulsadores y Display**:
   - Funciones como `bottonUp()`, `bottonDown()`, `bottonEnter()`, `bottonRight()` y `bottonLeft()` permiten manejar el incremento, decremento, reset y otros cambios en la variable `contador` usando los pulsadores.
   - La función `calcularFrecuencia()` calcula la frecuencia de la señal recibida.
   - `comprobarTeclado()` lee las teclas presionadas en el teclado matricial y almacena los números en una cadena.

5. **Manejo de Interrupciones**:
   - Las interrupciones generadas por el `TIMER3` permiten el manejo intercalado del display y el teclado en el `ISR(TIMER3_COMPA_vect)`.
   - Se utilizan diferentes condiciones en el `switch(digit)` para visualizar el valor del `contador` en los diferentes modos de operación.
   - La función `ISR(TIMER3_CAPT_vect)` se usa para medir el período de la señal de entrada y calcular la frecuencia.

### Notas importantes
   - Se recomienda monitorear los valores de `finalValue` y `frecuencia` para asegurar la precisión del frecuencímetro.
   - La multiplexación del display y el teclado en la interrupción del `TIMER3` permite ahorrar pines, pero requiere optimización en caso de escalabilidad.

Esta práctica es útil para familiarizarse con el manejo de periféricos básicos en un microcontrolador y la utilización de interrupciones para tareas concurrentes.
