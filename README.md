# Parcial domiciliario Sistemas de procesamiento de datos

## Integrantes Grupo 18
- Patricio Ballesteros
- Francisco Faig 

## Descripción
Este proyecto se da contexto del curso "Sistemas de Procesamiento de Datos" de la Universidad Tecnológica Nacional (UTN) - Facultad Regional Avellaneda.

El objetivo es la creación de un contador digital utilizando el microcontrolador Arduino UNO en la plataforma Tinkercad. 
A lo largo de tres etapas, se desarrollará un sistema que evoluciona desde un contador básico hasta una versión más compleja con funcionalidades extendidas.
El proyecto incluye tanto el diseño y montaje de componentes electrónicos como el desarrollo del software necesario para alcanzar los objetivos de cada fase.

## Parte 1: Contador Displays de 7 Segmentos y Multiplexación
En esta primera parte del proyecto, diseñamos un contador de 0 a 99 utilizando dos displays de 7 segmentos y tres botones para controlar la cuenta y implementando técnica de multiplexación. El contador comienza en 0 y es capaz de aumentar ,disminuir o regresar a 0 su valor usando los botones.
- Tinkercad: [link](https://www.tinkercad.com/things/8JuzkYb3aVi).
- Diagrama: [Link](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/diagramas/Parte%201.pdf).
![](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_1.PNG)


## Función Principal

### mostrarNumero 
```c
void mostrarNumero(int numero, int display)
{
  /*
  Muestra un numero en un display de 7 segmentos.

  Esta funcion permite mostrar numeros del 0 al 9 en dos displays diferentes (unidades y decenas).
  Controla la multiplexacion entre los displays para alternar entre mostrar el valor de
  las unidades y las decenas.

  Param
  - numero - El numero (0-9) que se mostrara en el display.
  - display - El display (unidades o decenas) en el que se mostrara el numero.

  Return
  - No tiene.
 */
  
  // Apaga todos los segmentos para evitar segmentos residuales.
  apagar();

  // Logica para designar en que display mostrar
  if (display == unidades)
  {
    // Cambiar a mostrar en el display de unidades
    digitalWrite(unidades, LOW);
    digitalWrite(decenas, HIGH);
    delay(tiempoDelay);
  }
  else if (display == decenas)
  {
    // Cambiar a mostrar en el display de decenas
    digitalWrite(decenas, LOW);
    digitalWrite(unidades, HIGH);
    delay(tiempoDelay);
  }

  // Mostrar el numero especificado en el display
  switch (numero)
  {
    case 0:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      break;
    case 1:
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    case2:
    .
    .
    .
    default:
      apagar();
      break;
   }
}

```
## Parte 2: Modificación con Interruptor Deslizante y Números Primos
En esta etapa, sustituimos uno de los botones por un switch deslizante de dos posiciones. 
Dependiendo la posiciondel interruptor, los display mostrarán el contador o los números primos en el rango de 0 a 99.
Ademas de eso, se agregaron una serie de componentes que amplian la funcionalidad:
  - Motor cc
    Se agrego un motor cc que aumenta o disminuye la velocidad en referencia al contador en ambos modos. Aplicacion de la funcion map().
  - Sensor de temperatura
    Interactua con bloques de codigo ligados a la inactividad. Si el programa detecta que no se toco ningun boton en un 
    timepo determinado, muestra una de tres animaciones en display que varian dependiendo de la temperatura.
  - Buzzer piezoelectrico (componente extra)
    Adicionalmente, se implemento un buzzer piezoelectrico y un switch para prenderlo y apagarlo. El piezo proporciona feedback sonoro al contador.
  
- Tinkercad: [link](https://www.tinkercad.com/things/giu3DfixEy2?sharecode=JLBI8ewCcy1uKQdBmG3tAXeHytqIhF4u6QK3HnRkYMQ).
- Diagrama: [Link](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/diagramas/Parte%202.pdf).
![](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_2.PNG)
