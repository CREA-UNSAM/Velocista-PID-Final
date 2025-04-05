# Proyecto de Seguidor de Línea con Control PID

Este proyecto implementa el control de un robot seguidor de línea utilizando sensores (digitales y analógicos) y control PID para ajustar la velocidad de los motores en función de la posición de la línea.

## Características

- **Detección de Línea:**  
  Utiliza sensores digitales y analógicos para detectar la línea. La constante `LINE_LOGIC` define la lógica (1 para línea negra y 0 para línea blanca).

- **Control PID:**  
  Implementa un controlador PID para corregir la posición del robot. Los parámetros PID (`Kp`, `Ki`, `Kd`) pueden ser modificados a través del monitor serie.

- **Control de Motores:**  
  Configura dos motores (izquierdo y derecho) con PWM y dos pines de dirección para cada motor. Se establecen límites máximos y mínimos para el PWM.

- **Modos de Operación:**  
  - **Modo Debug:** Muestra valores de sensores, posición de línea y salidas PID en el monitor serie para facilitar pruebas y ajustes.
  - **Modo Normal:** Controla el robot basándose en la lectura de sensores y actualiza la velocidad de los motores en intervalos regulares.
  - **Indicador LED:** Utiliza un LED para indicar el estado activo/inactivo del sistema, con parpadeo en modo inactivo.

- **Interacción con Usuario:**  
  Permite activar o desactivar el sistema mediante un botón físico y ajustar los parámetros PID mediante comandos enviados por el monitor serie.

## Estructura del Código

- **Configuración:**  
  Define constantes para la lógica de los sensores, parámetros del motor, intervalos de tiempo, pines de hardware y ajustes PID.

- **Estructuras de Datos:**  
  - `Motor`: Define los pines asociados al motor y la velocidad actual.
  - `PIDController`: Contiene los parámetros PID y variables internas para el cálculo.

- **Variables Globales:**  
  Variables para almacenar el estado de los sensores, los motores, el controlador PID y el estado del sistema.

- **Funciones Principales:**  
  - `setup()`: Inicializa el monitor serie, configura los pines de hardware y establece el modo de operación.
  - `loop()`: Bucle principal que ejecuta el control del robot. Dependiendo del modo (debug o normal) actualiza la lectura de sensores, realiza el cálculo PID y ajusta la velocidad de los motores.
  - `readSensors()`: Lee los valores de los sensores digitales y analógicos.
  - `calculateLinePosition()`: Calcula la posición de la línea basándose en los valores de los sensores y sus pesos.
  - `updatePID(double position)`: Calcula la corrección PID basada en la posición de la línea.
  - `setMotorSpeed(Motor &motor, int16_t speed)`: Ajusta la velocidad de un motor utilizando PWM y controla la dirección.
  - `toggleSystemState()`: Cambia el estado del sistema (activo/inactivo) al presionar el botón.
  - `handleSerialInput()`: Permite actualizar los parámetros PID mediante comandos enviados por el monitor serie.

## Requisitos de Hardware

- **Plataforma:** Arduino (cualquier modelo compatible con pines digitales y analógicos).
- **Sensores:**  
  - 2 sensores digitales para los extremos (SENSOR_D0 y SENSOR_D7).  
  - 6 sensores analógicos (SENSOR_A1 a SENSOR_A6).
- **Motores:**  
  - 2 motores DC controlados mediante PWM y pines de dirección.
- **LED y Botón:**  
  - LED para indicar el estado del sistema.  
  - Botón para alternar entre modos activo/inactivo.

## Cómo Usar

1. **Configuración del Hardware:**  
   - Conecta los sensores, motores, LED y botón a los pines especificados en el código.
   - Revisa las asignaciones de pines definidas en el `enum Pins` y ajusta si es necesario según tu montaje.

2. **Compilación y Carga:**  
   - Abre el código en el IDE de Arduino.
   - Compílalo y súbelo a la placa Arduino.

3. **Ajustes en Tiempo Real:**  
   - Abre el Monitor Serie a 9600 baudios para observar los valores en modo debug (si `DEBUG` está activado) o para enviar comandos.
   - Para actualizar los parámetros PID, escribe en el monitor serie:
     - `Kp<valor>` para ajustar la ganancia proporcional.
     - `Ki<valor>` para ajustar la ganancia integral.
     - `Kd<valor>` para ajustar la ganancia derivativa.

4. **Operación:**  
   - Usa el botón físico para alternar entre el estado activo e inactivo del sistema.
   - En estado activo, el robot seguirá la línea utilizando la lógica y los ajustes establecidos.

## Configuración y Ajustes

- **Parámetros del Motor:**  
  - `MOTOR_MAX_PWM` y `MOTOR_MIN_PWM` definen el rango de PWM.
  - `MOTOR_RIGHT_OFFSET` permite compensar diferencias en el rendimiento de los motores.

- **Velocidades e Intervalos:**  
  - `BASE_SPEED` define la velocidad base de los motores.
  - `RUN_INTERVAL` y `BLINK_INTERVAL` configuran los tiempos de ejecución del control y parpadeo del LED.

- **Ajustes PID:**  
  - Los parámetros `Kp`, `Ki` y `Kd` están definidos en la estructura `PIDController` y pueden ser modificados vía monitor serie para mejorar la respuesta del sistema.

## Notas Adicionales

- **Modo Debug:**  
  Si `DEBUG` está activado (`#define DEBUG 1`), el sistema no controlará los motores, sino que imprimirá datos útiles en el monitor serie para facilitar el ajuste y la depuración.
- **Debounce del Botón:**  
  Se implementa un debounce de 250ms en la función `toggleSystemState()` para evitar lecturas erráticas del botón.

---

