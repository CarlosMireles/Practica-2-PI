# Práctica 2: Timers en el Microcontrolador ATmega 2560

Este proyecto explora el uso de timers en el microcontrolador ATmega 2560, específicamente mediante el uso del Timer 3. Se enfoca en la generación de interrupciones internas para gestionar la visualización de un display y la implementación de un frecuencímetro.

## Objetivos

1. **Comprender el uso de los timers en el ATmega 2560**.
2. **Configurar y programar el Timer 3** para generar ondas rectangulares y manejar interrupciones.
3. **Implementar una señal PWM** para controlar la velocidad de motores o la intensidad de un LED.
4. **Medir la frecuencia de señales de entrada** usando el frecuencímetro implementado.

## Estructura del Proyecto

Este proyecto se basa en dos componentes principales:

1. **Control de Display y Teclado Matricial**: Utilizando interrupciones y multiplexación.
2. **Frecuencímetro**: Implementación de un sistema para medir la frecuencia de una señal de entrada.

### Archivos de Código

- **Código principal**: Programa que incluye la configuración del Timer 3, manejo de interrupciones y funciones para control de display y frecuencímetro.

## Configuración del Hardware

### Asignación de Pines

- **Pulsadores**: Conectados a `PORTC` para manejar los controles de incremento (`UP`), decremento (`DOWN`), entrada (`ENTER`), y ajustes (`LEFT`, `RIGHT`).
- **Teclado y Display**: El teclado matricial y el display de 7 segmentos están conectados a `PORTL`, compartiendo líneas de entrada/salida para optimizar el uso de pines.

### Salidas del Timer 3
- **OC3A**: Pin 5
- **OC3B**: Pin 2
- **OC3C**: Pin 3

Asegúrese de programar estos pines como salidas.

## Modos de Funcionamiento

El proyecto cuenta con cuatro modos de visualización, seleccionables desde la consola:

1. **Modo 1: Visualización Normal** - Muestra centenas, decenas y unidades.
2. **Modo 2: Visualización Reducida Inferior** - Solo muestra decenas y unidades.
3. **Modo 3: Visualización Reducida Superior** - Solo muestra centenas y decenas.
4. **Modo 4: Frecuencímetro** - Mide y muestra la frecuencia de una señal de entrada en Hz.

## Descripción de Funcionalidades

### Control de Display y Teclado

- **Multiplexación de Display**: Utiliza una interrupción generada cada 5 ms por el Timer 3 en modo CTC (Clear Timer on Compare Match). Cada dígito del display se activa de forma intercalada para optimizar el uso de pines.
- **Lectura de Teclado**: Las entradas del teclado matricial son gestionadas mediante el código en la interrupción, lo que permite capturar y almacenar los dígitos introducidos por el usuario.

### Frecuencímetro

- El frecuencímetro mide el tiempo entre dos flancos (usando el Timer 3 en modo de captura) y calcula la frecuencia.
- La función `calcularFrecuencia` convierte el tiempo medido en Hz para su visualización en el display.

### Manejo de Interrupciones

- **Interrupción TIMER3_COMPA_vect**: Activa cada 5 ms para el manejo intercalado del display.
- **Interrupción TIMER3_CAPT_vect**: Utilizada para capturar el tiempo entre flancos de la señal de entrada y calcular la frecuencia.

## Configuración del Timer 3

El Timer 3 se configura en modo CTC, donde el valor de TOP está determinado por el registro `OCR3A`. La configuración permite una interrupción cada 5 ms, lo cual facilita el manejo del display y el teclado de manera eficiente.

### Configuración de Registro

```cpp
cli();
TCCR3A = B01000000;
TCCR3B = B00001010;
OCR3A = 9999;       // TOP
sei();
```

## Referencias y Apéndices

Consulte la hoja de datos del microcontrolador ATmega 2560 para más detalles sobre los registros del Timer 3 y la configuración de los prescalers. Para más detalles sobre la configuración y uso del Timer 3, consulte las secciones de registros `TIFR3`, `TIMSK3` e `ICR3` en la documentación oficial.

## Autor

Carlos Mireles Rodríguez
