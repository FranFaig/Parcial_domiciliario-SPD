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

![Diagrama del circuito - Parte 1](https://github.com/FranFaig/Parcial_domiciliario-SPD/blob/main/imagenes/Parte_1.PNG)

- Tinkercad: [link](https://www.tinkercad.com/things/8JuzkYb3aVi).

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
