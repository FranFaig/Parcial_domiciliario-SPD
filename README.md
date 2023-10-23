# Parcial domiciliario Sistemas de procesamiento de datos

## Integrantes Grupo 18
- Patricio Ballesteros
- Francisco Faig 

## Descripción
Este proyecto se enmarca en el curso "Sistemas de Procesamiento de Datos" de la Universidad Tecnológica Nacional (UTN) - Facultad Regional Avellaneda.

El objetivo es crear un contador digital utilizando el microcontrolador Arduino UNO en la plataforma Tinkercad. A lo largo de tres etapas, se desarrollará un sistema que evoluciona desde un contador básico hasta una versión más compleja con funcionalidades extendidas. El proyecto incluye tanto el diseño y montaje de componentes electrónicos como el desarrollo del software necesario para alcanzar los objetivos de cada fase.

## Parte 1: Contador con Displays de 7 Segmentos y Multiplexación
En esta primera parte del proyecto, se diseñó un contador de 0 a 99 utilizando dos displays de 7 segmentos y tres botones para controlar la cuenta, implementando la técnica de multiplexación. El contador comienza en 0 y puede aumentar, disminuir o regresar a 0 su valor usando los botones.

- [Tinkercad](https://www.tinkercad.com/things/8JuzkYb3aVi).
- [Diagrama](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/diagramas/Parte%201.pdf).
- [Codigo](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/codigo/Parte_1.txt).

![](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_1.PNG)


### Función Principal
```c
 void loop()
  /*
  Loop principal del programa.

  Esta función se ejecuta continuamente y se encarga de manejar el contador de unidades
  y decenas, así como de actualizar la visualización en los displays de 7 segmentos. También
  detecta las interacciones con los botones de aumentar, disminuir y poner a cero.

  Nota: Esta función utiliza bandera de "debouncing" para evitar falsos positivos al
  presionar los botones.

  Parametros:
    Ninguno.
  Retorna:
    Ninguno.
*/
{
  // Leer el estado de los botones
  estadoBotonAumentar = digitalRead(BOTON_AUMENTAR);
  estadoBotonDisminuir = digitalRead(BOTON_DISMINUIR);
  estadoBotonCero = digitalRead(BOTON_CERO);

  // Mostrar los valores del contador en los displays
  mostrarNumero(contadorUnidades, unidades);
  mostrarNumero(contadorDecenas, decenas);

  // Comprobar si se ha presionado uno de los botones para evitar rebotes
  if (estadoBotonAumentar == HIGH && estadoBotonDisminuir == HIGH && estadoBotonCero == HIGH && botonYaApretado == true)
  {
    botonYaApretado = false;  // Reiniciar la bandera de boton presionado
    return;  
  }
  else if (estadoBotonAumentar == 0 && botonYaApretado == false)
  {
    
    botonYaApretado = true; // Marcar el botón como presionado para evitar multiples incrementos
    contadorUnidades++; // Incrementar el contador de unidades
    
    if (contadorUnidades > 9)
    {
      contadorUnidades = 0;
      contadorDecenas++;// Cuando las unidades llegan a 0, incrementar el contador de decenas
      mostrarNumero(contadorDecenas, decenas);
      if (contadorDecenas > 9)
      {
        contadorDecenas = 0;
      }
    }
    
  }else if (estadoBotonDisminuir == 0 && botonYaApretado == false)
  {
    // Marcar el boton como presionado para evitar múltiples decrementos
    botonYaApretado = true;

    // Decrementar el contador de unidades
    contadorUnidades--;

    if (contadorUnidades < 0) 
    {
      contadorUnidades = 9;
      contadorDecenas--; // Cuando las unidades llegan a 9, decrementar el contador de decenas
      if (contadorDecenas < 0)
      {
        contadorDecenas = 9;
      }
    }
  }else if (estadoBotonCero == 0 && botonYaApretado == false)
  {
    // Marcar el botón como presionado para evitar multiples reinicios
    botonYaApretado = true;
    // Reiniciar ambos contadores a cero
    contadorUnidades = 0;
    contadorDecenas = 0;
  }
}
```

## Parte 2: Modificación con Interruptor Deslizante y Números Primos
En esta etapa, sustituimos uno de los botones por un switch de dos posiciones. Dependiendo de la posición del interruptor los displays mostrarán el contador o los números primos en el rango de 0 a 99. Además, se agregaron una serie de componentes que amplían la funcionalidad:

- Motor de corriente continua (CC): Se agregó un motor de CC que aumenta o disminuye la velocidad en referencia al contador en ambos modos. Se aplicó la función map().
- Sensor de temperatura: Interactúa con bloques de código relacionados con la inactividad. Si el programa detecta que no se tocó ningún botón en un tiempo determinado, muestra una de tres animaciones en el display que varían dependiendo de la temperatura.
- Buzzer piezoeléctrico (componente adicional): Tambien, se implementó un buzzer piezoeléctrico y un switch para encenderlo y apagarlo. El piezo proporciona feedback sonoro al contador.

- [Tinkercad](https://www.tinkercad.com/things/giu3DfixEy2?sharecode=JLBI8ewCcy1uKQdBmG3tAXeHytqIhF4u6QK3HnRkYMQ).
- [Diagrama](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/diagramas/Parte%202.pdf).
- [Codigo](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/codigo/Parte_2.txt).

![](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_2.PNG)

### Cambios adicionales.
- Implementación de funciones: mostrarSegmento(), mostrarPatron(), esPrimo().
- Despliegue de la lógica de aumento y disminución en sus propias funciones: mostrarSiguiente() y mostrarAnterior(), respectivamente.
- Implementación de un sistema de detección de inactividad.
- Desarrollo de 3 animaciones de inactividad con uso de attachInterrupt().
- Nuevo sistema de "debouncing" con funciones handlers.


### Función Principal
```c
void loop() {
  /*
  El loop principal del programa.
  
  Se realiza el seguimiento de la temperatura, se controla el motor,
  se manejan los interruptores de modo, se actualiza el estado del display, y se verifican
  los botones de aumento y disminucion. Tambien se activan animaciones si no hay interacciones.
  
  */
  int datosSensorTemp = analogRead(SENSOR_TEMP);  // Lee la temperatura desde el sensor.
  float temperatura = map(datosSensorTemp, 20, 358, -40, 125);  // Mapea el valor del sensor al rango de -40 a 125 grados.

  // Lee el estado de los interruptores.
  estadoSwitchPiezo = digitalRead(SWITCH_PIEZO);
  estadoSwitchModo = digitalRead(SWITCH_MODO);

  // Mapea el valor del numero actual al rango del motor.
  int motorSpeed = map(numeroActual, 0, 99, 0, 255);
  analogWrite(PIN_MOTOR, motorSpeed);

  // Configura el modo actual basado en el interruptor de modo.
  if (estadoSwitchModo == HIGH) {
    modoActual = CONTADOR;
  } else {
    modoActual = PRIMOS;
  }

  // Activa animaciones si no hay interaccion durante un tiempo especifico.
  if (millis() - ultimaInteraccion >= limiteInactividad) {
    if (temperatura >= -40 && temperatura < 20) {
      mostrarAnimCirculo();
    } else if (temperatura >= 20 && temperatura < 75) {
      mostrarAnimSerpiente();
    } else if (temperatura >= 75 && temperatura <= 150) {
      mostrarAnimCuadrados();
    }
  }

  // Verifica si el boton de aumentar fue precionado.
  if (botonAumentarPresionado) {
    mostrarSiguiente(modoActual, numeroActual);
    botonAumentarPresionado = false;
  }

  // Verifica si el boton de disminuir fue precionado.
  if (botonDisminuirPresionado) {
    mostrarAnterior(modoActual, numeroActual);
    botonDisminuirPresionado = false;
  }

  // Muestra los numeros en los displays.
  mostrarNumero(contadorUnidades, DISPLAY_UNIDADES);
  mostrarNumero(contadorDecenas, DISPLAY_DECENAS);
}
```

## Parte 3: Modificación según el Último Número de Documento
En esta última etapa, agregamos un componente adicional que fue designado según el número de documento, para proporcionar una nueva funcionalidad al proyecto. El componente implementado fue una fotoresistencia.

Utilizamos la fotoresistencia como regulador de volumen del buzzer piezoeléctrico. De esta manera, el piezo sonará más fuerte o más despacio basado en el nivel de luz. 
No se realizaron más modificaciones en esta fase.


- [Tinkercad](https://www.tinkercad.com/things/e3zn10geIr3).
- [Diagrama](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/diagramas/Parte%203%20.pdf).
- [Codigo](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/codigo/Parte_3.txt).

![](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_3.PNG)


