<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>Curso de Robótica 2026-I</h3>

<h1>Proyecto Final - Robótica Industrial</h1>

<h2>Profesores: <br>Pedro Fabián Cárdenas Herrera <br> Manuel Felipe Carranza Montenegro<br></h2>

</div>

# Integrantes
1. Juan Andrés Moreno Benavides [jumorenobe@unal.co](Jumorenobe)
2. Mateo Ramos Cujer [mramoscu@unal.edu.co](MateoKGR)
3. Paula Valentina Alvarez Chaparro [palvarezch@unal.edu.co] (Vaulentzc)
4. Yesid Camilo Ojeda Hernández [yojeda@unal.edu.co] (YojedaMt)
5. David Felipe Cardenas Cubides [dcardenascu@unal.edu.co] (dcardenascu)

# Índice

1. [Bitácora del desarrollo](#bitacora-del-desarrollo)
2. [Diagrama de flujo del proceso global](#diagrama-de-flujo-del-proceso-global)
3. [Diseño del gripper](#diseño-del-gripper)
4. [Simulación](#simulacion)
5. [Código fuente](#codigo-fuente)
6. [Comparación manual vs automatizado](#comparacion-manual-vs-automatizado)
7. [Descripción detallada de la solución](#descripcion-detallada-de-la-solución)
8. [Diagrama de flujo acciones del robot](#diagrama-de-flujo-acciones-del-robot)
9. [Plano de planta](#plano-de-planta)
10. [Videos demostrativos](#videos-demostrativos)

## 1. Bitácora del desarrollo
## 2. Diagrama de flujo del proceso global
## 3. Diseño del gripper
## 4. Simulación

## 5. Código fuente
El control y automatización de la **Etapa 2: Ensamblaje de Componentes** ejecutada por el robot **Epson T3 401S ("Junior")**  se desarrolló en el entorno **EPSON RC+** empleando el lenguaje de programación **SPEL+**. 

A continuación se detalla la estructura y funcionamiento del código implementado para la manipulación y posicionamiento de componentes mediante el electroimán adaptado.

### Estructura del Código

#### 1. Declaración de Variables Globales
```i```: Variable de control para los iteradores en los bucles de ensamble.
```alm(20)```: Arreglo que almacena las posiciones relativas (índices) de cada componente dentro del almacén ordenado.
```baq(20)```: Arreglo que asigna la posición relativa destino dentro de la tarjeta electrónica (baquela/PCB).
NumComponentes: Determina el número total de piezas a manipular durante la ejecución.

### 2. Rutina Principal (main)
Es el punto de entrada del programa. Establece la configuración de seguridad, velocidad del manipulador y coordina la secuencia global del ciclo de trabajo.
```Power High / Accel / Speed```: Habilita la máxima potencia y fija las velocidades e inercias al 100% para optimizar el tiempo de ciclo.
```Home```: Envía el robot a su posición segura inicial y final para prevenir colisiones durante el encendido o parada.
Llamado a subrutinas: Ejecuta de manera secuencial la asignación de posiciones y el proceso de pick & place.

### 3. Mapeo de Componentes (CargarComponente)
Define la relación entre el origen (almacén) y el destino (baquela) para cada ítem de la receta de ensamble.

### 4. Proceso de Ensamble y Paletizado (EnsambleComponentes)
Configura las mallas de trabajo (pallets) y ejecuta la rutina iterativa de Pick & Place utilizando trayectorias arqueadas para garantizar la seguridad en la inserción.

```Pallet 1```: Define la rejilla del Almacén con una matriz de $6 \times 2$ posiciones.
```Pallet 2```: Define la rejilla de la Baquela con una matriz de $27 \times 22$ posiciones.
```Jump```: Ejecuta un movimiento tipo SCARA Gate (eleva el eje Z, desplaza X/Y y baja Z), evitando obstáculos intermedios entre la zona de almacenamiento y la PCB.

### 5. Control del Actuador Final (Tomar / Soltar)
Gestión del efector final (electroimán) a través de salidas digitales del controlador Epson.

```DO_09```: Salida digital conectada al relé de activación del electroimán. Funciona bajo lógica invertida (o normalmente cerrado según el arreglo físico del relé):
```Off DO_09```: Energiza/activa la sujeción electromagnética para tomar el componente.
```On DO_09```: Desenergiza la herramienta para liberar el componente sobre la PCB.

## 6. Comparación manual vs automatizado
## 7. Descripción detallada de la solución
## 8. Diagrama de flujo acciones del robot
El siguiente diagrama detalla la secuencia algorítmica y la toma de decisiones que ejecuta el robot  durante la **Etapa 2: Ensamblaje de Componentes**, reflejando la lógica programada en **SPEL+**:

```mermaid
flowchart TD
    A([Inicio: main]) --> B[Habilitar Motores: Motor On]
    B --> C[Configurar Dinámica: Power High, Speed 100, Accel 100, 100]
    C --> D[Mover a Posición de Seguridad: Home]
    
    D --> E[Ejecutar: CargarComponentes]
    E --> F[Definir NumComponentes]
    F --> G[Mapear Arreglos: alm i -> Posición Almacén<br/>baq i -> Posición Baquela]
    
    G --> H[Ejecutar: EnsambleComponentes]
    H --> I["Configurar Rejillas:<br/>Pallet 1 (Almacén 6x2)<br/>Pallet 2 (Baquela 27x22)"]
    I --> J[Inicializar Contador: i = 1]
    
    J --> K{¿i <= NumComponentes?}
    
    K -- Sí --> L["Jump Pallet(1, alm(i))<br/>Desplazamiento tipo arco a la celda del almacén"]
    L --> M[Wait 1s]
    M --> N["Call Tomar<br/>Off DO_09 (Energiza Electroimán)"]
    N --> O[Wait 1s]
    
    O --> P["Jump Pallet(2, baq(i))<br/>Desplazamiento tipo arco hacia la PCB"]
    P --> Q[Wait 1s]
    Q --> R["Call Soltar<br/>On DO_09 (Desenergiza Electroimán)"]
    R --> S[Wait 1s]
    
    S --> T[Incrementar: i = i + 1]
    T --> K
    
    K -- No --> U[Mover a Posición Final: Home]
    U --> V([Fin del Programa: Fend])

    %% Estilos visuales del diagrama
    style A fill:#4CAF50,stroke:#333,stroke-width:2px,color:#fff
    style V fill:#F44336,stroke:#333,stroke-width:2px,color:#fff
    style K fill:#FF9800,stroke:#333,stroke-width:2px,color:#fff
    style N fill:#2196F3,stroke:#333,stroke-width:2px,color:#fff
    style R fill:#9C27B0,stroke:#333,stroke-width:2px,color:#fff

```

## 9. Plano de planta
## 10. Videos demostrativos