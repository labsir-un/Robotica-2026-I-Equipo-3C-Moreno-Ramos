<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>Curso de Robótica 2026-I</h3>

<h1>Informe Laboratorio #4</h1>

<h2>Profesores: <br>Pedro Fabián Cárdenas Herrera <br> Manuel Felipe Carranza Montenegro<br></h2>

</div>

# Integrantes
1. Juan Andrés Moreno Benavides [jumorenobe@unal.co](Jumorenobe)
2. Mateo Ramos Cujer [mramoscu@unal.edu.co](MateoKGR)

# Índice

- [1. Descripción General](#1-descripción-general)
- [2. Arquitectura de ROS 2 (Nodos, Tópicos y Servicios)](#2-arquitectura-de-ros-2-nodos-tópicos-y-servicios)
- [3. Control Manual de la Tortuga)](#3-control-manual-de-la-tortuga)
- [4. Funciones Automáticas y Trazado de Iniciales](#4-funciones-automáticas-y-trazado-de-iniciales)
- [5. Sistema Líder-Seguidor](#5-sistema-líder-seguidor)
- [6. Diagrama de Flujo del Sistema](#6-diagrama-de-flujo-del-sistema)
- [7. Verificación de la Arquitectura (Comandos de Inspección)](#7-verificación-de-la-arquitectura-comandos-de-inspección)
- [8. Evidencias de Funcionamiento](#8-evidencias-de-funcionamiento)
- [9. Video Explicativo y Conclusiones](#9-video-explicativo-y-conclusiones)

---

## 1. Descripción General

Este laboratorio práctico consiste en el diseño, desarrollo e implementación de un sistema de control robótico multiagente utilizando **ROS 2 Jazzy Jalisco** sobre el sistema operativo **Ubuntu 24.04 LTS**. El proyecto explora los fundamentos de la comunicación distribuida en robótica mediante el simulador bidimensional `turtlesim`.

El núcleo del desarrollo se divide en tres hitos principales:
* **Teleoperación de bajo nivel:** Creación de un sistema de lectura directa del teclado en bruto (*raw mode*) sin depender de herramientas estándar como `turtle_teleop_key`, garantizando un control asíncrono y libre de bloqueos.
* **Control cinemático y navegación por puntos (Waypoints):** Implementación de una estrategia híbrida que combina el control por tiempo para figuras geométricas regulares (cuadrado y triángulo) y un control en bucle cerrado basado en coordenadas absolutas para el trazado de iniciales personalizadas de alta precisión.
* **Comportamiento Multiagente (Líder-Seguidor):** Desarrollo de un controlador descentralizado donde un robot seguidor (`turtle2`) procesa la telemetría en tiempo real de un robot líder (`turtle1`) para realizar una aproximación progresiva y suave mediante un controlador proporcional, respetando distancias críticas de seguridad.


