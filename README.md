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
En esta primera parte del proyecto, diseñamos un contador de 0 a 99 utilizando dos displays de 7 segmentos y tres botones para controlar la cuenta e implementado técnica de multiplexación. El contador comienza en 0 y es capaz de aumentar ,disminuir o regresar a 0 su valor usando los botones.

![Diagrama del circuito - Parte 1]()

- Parte 1 en Tinkercad [aquí](https://www.tinkercad.com/things/8JuzkYb3aVi).

## Función PrincipaL

### `mostrarNumero(int numero, int display)`
La funcion mostrarNumero brinda la mayor parte de la funconalidad.
Permite mostrar números del 0 al 9 en dos displays diferentes (unidades y decenas) y controla la multiplexación entre los displays.
