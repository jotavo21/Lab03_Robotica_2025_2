# Laboratorio 03 Robótica - Robótica Industrial - Análisis y Operación del Manipulador EPSON T3-401S

## Presentado por Juan Esteban Otavo García y Ian Saonni Rodríguez Pulido

En este laboratorio se estudiaron las funciones básicas del manipulador industrial EPSON T3-401S utilizando el software EPSON RC 7.0+. Para ello, se programó una rutina que permitiera al manipulador mover dos huevos de forma intercalada, replicando el movimiento del caballo de ajedrez dentro de una matriz de posiciones de 5×6 (una cubeta de huevos común). El objetivo fue que cada uno de los huevos recorriera las 30 posiciones de la cubeta.

## Cuadro comparativo entre manipulador Motoman MH6, ABB IRB140 y EPSON T3-401S

| **Característica** | **Motoman MH6 (Yaskawa)** | **ABB IRB 140 (ABB Robotics)** | **EPSON T3-401S (Epson Robotics)** |
|---------------------|---------------------------|--------------------------------|-------------------------------------|
| **Carga máxima (payload)** | 6 kg | 6 kg | 3 kg |
| **Alcance máximo** | 1424 mm | 810 mm | 400 mm (brazo SCARA) |
| **Número de ejes / grados de libertad** | 6 ejes | 6 ejes | 4 ejes (SCARA) |
| **Repetibilidad** | ± 0.08 mm | ± 0.03 mm | ± 0.02 mm |
| **Peso del robot** | 130 kg aprox. | 98 kg | 14 kg aprox. |
| **Velocidad eje 1 (base)** | 170 °/s | 250 °/s | Hasta 450°/s |
| **Velocidad eje 2** | 160 °/s | 230 °/s | Hasta 720°/s |
| **Velocidad eje 3** | 170 °/s | 260 °/s | Eje vertical hasta 700 mm/s |
| **Velocidad eje 4 (muñeca/rotación herramienta)** | 340 °/s | 320 °/s | ± 1500 °/s |
| **Velocidad eje 5** | 340 °/s | 320 °/s | N/A |
| **Velocidad eje 6** | 520 °/s | 420 °/s | N/A |
| **Tipo de montaje** | Piso, pared o invertido | Piso, pared, invertido o en ángulo | Piso o mesa |
| **Controlador** | DX100 / DX200 | IRC5 Compact | EPSON T-Series All-in-One Controller |
| **Fuente de alimentación** | 200–230 V AC trifásico | 200–600 V AC trifásico | 100–240 V AC monofásico |
| **Grado de protección** | IP54 (cuerpo) / IP67 (muñeca) | IP54 (cuerpo) / IP67 (muñeca) | IP20 |
| **Rango de temperatura de operación** | 0–45 °C | 5–45 °C | 5–40 °C |
| **Flange (montaje de herramienta)** | ISO 9409-1-A50 | ISO 9409-1-A50 | ISO 9409-1-31.5 |
| **Aplicaciones típicas** | Manipulación general, soldadura, carga/descarga, ensamblaje, paletizado | Ensamblaje de precisión, manipulación, laboratorio, atornillado | Pick & place, electrónica, empaques, laboratorio |
| **Ventajas destacadas** | Gran alcance y estructura robusta | Alta precisión y tamaño compacto | Ligero, rápido, eficiente y fácil de instalar |
| **Limitaciones** | Repetibilidad menor que IRB 140 | Alcance corto | Carga y alcance reducidos; no para trabajos pesados |

##	Descripción de las configuraciones HOME del EPSON T3-401S, indicando la posición de cada articulación.

EN el caso del Robot de Epson, no viene una posición HOME predefinida, es más bien, una posición de referencia definida por el usuario dentro del software Epson RC+. Permite, entonces, adaptar este punto de referencia a las necesidades específicas de cada aplicación. Su configuración se realiza directamente desde el Robot Manager del software Epson RC+, donde es posible ver los valores de cada articulación en pulsos de codificador, asignar la posición actual del robot como nuevo HOME y modificarla en cualquier momento. Además, el sistema permite definir el orden de movimiento de las articulaciones cuando el robot se dirige al HOME, garantizando que el retorno a esta posición se realice de manera segura y coherente con la disposición física de la célula de trabajo.


## Modo de operación de movimientos manuales

En el entorno de programación EPSON RC+ 7.0+, los movimientos manuales del robot se llevan a cabo desde la ventana Robot Manager, específicamente en la sección Mover y enseñar. Cuando el usuario accede a esta pestaña, el sistema ofrece diferentes modos de operación que determinan cómo interpreta el robot las órdenes de movimiento manual. Los dos modos más comunes son el modo articular (Modo Articulación) y el modo cartesiano (Modo Mundo/Herramienta). En el primero, los desplazamientos afectan directamente a los ejes del robot  J1, J2, J3 y J4, permitiendo girar cada articulación de forma independiente. En contraste, en el modo cartesiano el movimiento se produce siguiendo las coordenadas espaciales del efector final: los ejes X, Y y Z para traslaciones, y los ejes RX, RY y RZ para rotaciones. De esta manera, el desplazamiento del robot responde a una representación espacial absoluta o relativa, dependiendo de si se trabaja en el sistema de referencia del mundo (World Frame) o del efector (Tool Frame).

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Imagenes/Moveryensenar.png)

El cambio entre estos modos se realiza directamente en la parte superior del panel Mover, donde la interfaz muestra el campo Modo, a través del cual es posible seleccionar Articulación, Mundo, Herramienta, Local o ECP. Al elegir Articulación, la barra de mandos muestra únicamente los controles asociados a cada articulación, permitiendo mover el robot incrementando o disminuyendo el ángulo de cada eje. Al seleccionar Mundo o Herramienta, la interfaz cambia para permitir movimientos en el espacio cartesiano, mostrando botones o controles asociados a los ejes X, Y, Z y a sus rotaciones correspondientes. 

Una vez establecido el modo de operación, el usuario puede mover el robot ejecutando traslaciones en los ejes cartesianos X, Y y Z o moviendo cada articulación de forma independiente. La ejecución de cada movimiento puede realizarse de forma continúa o de forma incremental dependiendo de la configuración seleccionada en Distancia de Movimiento (Continuo, Largo, Medio y Corto).


## Niveles de velocidad para movimientos manuales

En el software EPSON RC+ 7.0+, los movimientos manuales del robot se realizan utilizando dos niveles de velocidad predefinidos: Bajo y Alto. Estos niveles determinan la rapidez con la que se desplaza el robot cuando se mueve mediante las herramientas presentes en la pestaña de configuración del robot, lo que hace las veces de teach pendant. El nivel Low corresponde a una velocidad reducida, adecuada para aproximaciones finas y trabajos en zonas cercanas a obstáculos o al propio operador, mientras que el nivel High permite movimientos más rápidos, útiles cuando el robot se encuentra lejos de elementos de riesgo. Aun cuando se seleccione el nivel alto, los movimientos manuales siguen ejecutándose en modo de potencia baja.

Para cambiar entre los niveles Low y High, se debe ingresar al Robot Manager (presionando F6), luego se selecciona la pestaña Mover y enseñar. En esta ventana se encuentra la subsección denominada Mover, dentro del cual aparece el campo Velocidad. Este campo contiene un menú desplegable en el que se puede seleccionar directamente el nivel deseado. El ajuste elegido se aplica inmediatamente a todos los desplazamientos manuales que se realicen desde esa misma interfaz, incluyendo los movimientos continuos y los movimientos paso a paso asociados a la opción distancia de movimiento.

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Imagenes/Moveryensenar.png)

##	Descripción de las principales funcionalidades de EPSON RC+ 7.0, comunicación y procesos para ejecutar movimientos.
A continuación se van a describir las principales funciones del Software tratadoe en este laboratorio, estas hacen referencia a las que se encuentran en la barra superior y que, de alguna u otra manera, interactuan directamente con el usuario y el Robot. 
-Administrador de Robot (Robot Manager): Ventana central para ver y gestionar el estado del robot y sus parámetros; desde allí puedes abrir paneles de control, encender/apagar motores, editar y seleccionar dispositivos de control, ver listas de tareas y acceder a herramientas relacionadas con el controlador. Es el lugar desde el que se configuran y supervisan aspectos generales del manipulador. 

-Abrir Ventana de Comando (Command Window / “Command”): Consola de comandos directa para enviar órdenes al controlador (por ejemplo MOTOR ON, PULSE, MOV, etc.), ejecutar macros y ver respuestas en texto. Es muy útil para pruebas rápidas, secuencias de arranque y diagnóstico inmediato. 

-Monitor E/S (I/O Monitor): Muestra en tiempo real el estado de entradas y salidas digitales/analógicas del controlador; permite forzar una salida, comprobar etiquetas de E/S asignadas y verificar señales de sensores y actuadores durante la puesta en marcha o la depuración. 

-ForceGuide: Módulo/solución de guía por fuerza de Epson (opcional) que añade detección y control de fuerzas en seis ejes; se usa para tareas que requieren tacto fino (ensamblajes, inserciones, control por contacto) y permite guiar el movimiento según lecturas de fuerza/torque. 

-Monitor de Fuerza (Force Monitor): Pantalla y herramientas asociadas a ForceGuide que muestran lecturas de fuerza/torque, permiten configurar umbrales y activar rutinas de control por fuerza; útil para ajustar tolerancias y detectar colisiones o contactos fuera de lo esperado. 

-Administrador de Tareas (Task Manager): Permite ver y controlar las tareas (threads) del robot: arrancar, parar, cambiar prioridad y depurar múltiples tareas SPEL+ simultáneas. Es la herramienta para gestionar concurrencia y flujos lógicos del programa del robot. 

-Editor de Etiquetas E/S (I/O Label Editor / Editor de etiquetas E/S): Interfaz para crear/editar nombres amigables (etiquetas) para bits de entrada/salida; estas etiquetas luego aparecen en el I/O Monitor y en los programas, facilitando la lectura y evitando usar números crípticos de bit. 

-Simulador (Simulator / 3D Simulator): Simulación 3D integrada para crear y validar la celda, probar programas offline, detectar colisiones, estimar tiempos de ciclo y verificar comportamiento sin tocar el robot físico. Permite “teach” virtual y conectar visión virtual para depurar la aplicación antes de instalar hardware. 

-GUI Builder: Herramienta opcional para crear interfaces gráficas (formularios, botones, displays) integradas en RC+; sirve para construir pantallas de operador personalizadas (HMI) sin necesidad de herramientas externas, y conectarlas a variables/etiquetas del proyecto.

## Análisis comparativo entre EPSON RC+ 7.0, RoboDK y RobotStudio

### 1. EPSON RC+ 7.0

EPSON RC+ 7.0 es el entorno de programación nativo para los robots EPSON. Es un software diseñado para ofrecer una integración completa entre programación, simulación y control, priorizando la precisión y la estabilidad del funcionamiento real para los manipuladores de la compañia.

#### **Ventajas**
- Integración nativa con robots EPSON, lo que garantiza una alta correspondencia entre la simulación y el comportamiento real del manipulador.
- Compatibilidad directa con el controlador EPSON T-Series y otros controladores de la familia RC.
- Lenguaje SPEL+, optimizado para operaciones repetitivas y de alta velocidad.
- Herramientas integradas para visión artificial, configuración de E/S y calibración del sistema.

#### **Limitaciones**
- Funciona exclusivamente con robots EPSON, lo que restringe su utilidad en entornos multimarca.
- La capacidad de simulación es menor en comparación con plataformas avanzadas como RobotStudio.
- Menor profundidad analítica en trayectorias complejas o procesos de alto grado de personalización.

#### **Aplicaciones**
- Ensamblaje electrónico.
- Tareas repetitivas de alta velocidad.
- Laboratorios de automatización ligera.
- Procesos de Pick and Place de elementos pequeños.


### 2. RoboDK

RoboDK es una plataforma versátil orientada a la simulación de robots de diversos fabricantes. Se destaca por su flexibilidad y su capacidad de integrar múltiples marcas en un solo entorno.

#### **Ventajas**
- Plataforma multimarca que soporta robots de Yaskawa, ABB, Fanuc, KUKA, UR, Staubli, entre otros.
- Permite visualizar el desempeño de robots de diferentes fabricantes dentro de un mismo entorno.
- Incluye módulos CAD/CAM, facilitando la importación de trayectorias y geometrías complejas.
- Licencia básica gratuita y costos de licencia accesibles.
- Interfaz flexible que permite generar código para múltiples lenguajes y controladores.

#### **Limitaciones**
- La precisión de la simulación es menor que la obtenida en software nativo de cada fabricante.
- No replica el comportamiento interno de los controladores reales; el código debe ser ajustado sobre el robot físico.
- Dependencia de modelos cinemáticos genéricos, lo que puede generar diferencias con el robot real.

#### **Aplicaciones**
- Procesos CAD/CAM y mecanizado automatizado.
- Investigación académica y formación en robótica industrial.
- Integraciones multimarca y comparaciones de desempeño.

---

### 3. RobotStudio

RobotStudio es el software de simulación oficial de ABB Robotics, considerado uno de los entornos más avanzados a nivel industrial gracias al uso del Virtual Controller.

#### **Ventajas**
- Desarrollado por ABB Robotics con compatibilidad exclusiva para robots ABB.
- Incorpora el Virtual Controller, una réplica exacta del controlador físico IRC5/Omnicore, garantizando una simulación altamente precisa.
- Detecta errores reales de programación y reproduce trayectorias con alta fidelidad.
- Integración completa con el lenguaje RAPID, facilitando la transferencia directa de programas.
- Herramientas avanzadas para análisis de colisiones, tiempos de ciclo y puesta en marcha virtual.

#### **Limitaciones**
- Solo funciona con robots ABB, lo que limita su aplicabilidad en entornos mixtos.
- Requiere mayor capacidad de hardware debido a la complejidad gráfica y analítica.
- Curva de aprendizaje más alta, especialmente para usuarios sin experiencia en RAPID.

#### **Aplicaciones**
- Programación offline profesional.
- Simulación avanzada y optimización de celdas ABB.
- Diagnóstico de errores y pruebas de integración antes de la puesta en marcha.
- Entrenamiento especializado para operadores y programadores ABB.

## Diseño herramienta para sostener el gripper

Para sostener el gripper, se tiene el siguiente acople de aluminio en el laboratorio (planos anexos):

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Imagenes/AcopleGripper.png)


Dicho acople sostiene el gripper sin problema, por lo que se diseñó una herramienta que pudiera soportar el acople. Para esto, se propuso la construcción de una herramienta de tipo arandela que utilizando tuercas se pueda insertar a presión en el brazo articular del robot. La herramienta diseñada es la siguiente (planos anexos):

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Imagenes/Herramienta.png)

## Código utilizado para controlar el Manipulador

El código corresponde a la rutina principal que controla el manipulador para mover dos huevos en L dentro una cubeta de 5×6 posiciones. Al inicio se declara una variable global i de tipo entero (Global Integer i). La función main define la secuencia de trabajo del robot: primero se encienden los motores con Motor On, se establece el nivel de potencia en Power High y se configuran parámetros generales de movimiento mediante Accel 100, 100 (aceleración y desaceleración) y Speed 100 (velocidad base de los movimientos), ambos parámetros de movimiento se configuran dando un valor de 0 a 100, como un porcentaje. A continuación, el comando Home lleva al robot a la posición configurada como Home.

Luego, se utliza la instrucción Pallet 1, Origen, PuntoX, PuntoY, 5, 6. Aquí se define una malla de 30 posiciones (5 filas por 6 columnas) donde el punto Origen corresponde a la esquina de referencia de la primera posición, mientras que PuntoX y PuntoY definen otros dos extremos que permiten definir el plano donde se contendrán las 30 posiciones equidistantes. A partir de estos tres puntos, el controlador calcula automáticamente la posición cartesiana de cada celda del pallet, numerándolas de 1 a 30. De esta forma, la llamada Pallet(1, n) devuelve la posición del huevo número n dentro de la cuadrícula, sin necesidad de programar manualmente cada coordenada.

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Imagenes/CoordenadasPallet.jpg)
 
Posteriormente, se utiliza la función Jump Pallet (1, n), para saltar entre diferentes posiciones, se tienen los siguientes arreglos con las posiciones de cada huevo.

A = [1, 12, 21, 28, 25, 14, 5, 8, 19, 10, 3, 6, 13, 22, 29, 20, 9, 2, 11, 18, 27, 16, 7, 4, 15, 24, 17, 26, 23, 30]

B = [30, 19, 10, 3, 6, 17, 26, 23, 14, 5, 8, 1, 12, 21, 28, 25, 18, 29, 20, 9, 2, 11, 22, 13, 16, 27, 24, 15, 4, 7]

Y finalmente, se conmuta el estado de la salida Out_9 para encender y apagar el gripper a conveniencia, dejando un delay de 0.5s usando Wait 0.5 para asegurarse que el gripper logre tomar correctamente el huevo.

```vb
Global Integer i
Function main
    Motor On
    Power High
    Accel 100, 100
    Speed 100
    Home
    Pallet 1, Origen, PuntoX, PuntoY, 5, 6
     
    Jump Pallet(1, 1)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 12)
    On Out_9

    Jump Pallet(1, 30)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 19)
    On Out_9

    Jump Pallet(1, 12)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 21)
    On Out_9

    Jump Pallet(1, 19)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 10)
    On Out_9

    Jump Pallet(1, 21)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 28)
    On Out_9

    Jump Pallet(1, 10)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 3)
    On Out_9

    Jump Pallet(1, 28)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 25)
    On Out_9

    Jump Pallet(1, 3)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 6)
    On Out_9

    Jump Pallet(1, 25)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 14)
    On Out_9

    Jump Pallet(1, 6)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 17)
    On Out_9

    Jump Pallet(1, 14)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 5)
    On Out_9

    Jump Pallet(1, 17)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 26)
    On Out_9

    Jump Pallet(1, 5)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 8)
    On Out_9

    Jump Pallet(1, 26)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 23)
    On Out_9

    Jump Pallet(1, 8)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 19)
    On Out_9
    
    Jump Pallet(1, 23)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 14)
    On Out_9

    Jump Pallet(1, 19)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 10)
    On Out_9

    Jump Pallet(1, 14)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 5)
    On Out_9

    Jump Pallet(1, 10)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 3)
    On Out_9

    Jump Pallet(1, 5)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 8)
    On Out_9

    Jump Pallet(1, 3)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 6)
    On Out_9

    Jump Pallet(1, 8)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 1)
    On Out_9

    Jump Pallet(1, 6)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 13)
    On Out_9

    Jump Pallet(1, 1)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 12)
    On Out_9

    Jump Pallet(1, 13)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 22)
    On Out_9

    Jump Pallet(1, 12)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 21)
    On Out_9

    Jump Pallet(1, 22)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 29)
    On Out_9

    Jump Pallet(1, 21)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 28)
    On Out_9

    Jump Pallet(1, 29)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 20)
    On Out_9

    Jump Pallet(1, 28)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 25)
    On Out_9

    Jump Pallet(1, 20)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 9)
    On Out_9

    Jump Pallet(1, 25)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 18)
    On Out_9

    Jump Pallet(1, 9)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 2)
    On Out_9

    Jump Pallet(1, 18)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 29)
    On Out_9

    Jump Pallet(1, 2)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 11)
    On Out_9

    Jump Pallet(1, 29)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 20)
    On Out_9

    Jump Pallet(1, 11)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 18)
    On Out_9

    Jump Pallet(1, 20)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 9)
    On Out_9

    Jump Pallet(1, 18)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 27)
    On Out_9

    Jump Pallet(1, 9)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 2)
    On Out_9

    Jump Pallet(1, 27)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 16)
    On Out_9

    Jump Pallet(1, 2)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 11)
    On Out_9

    Jump Pallet(1, 16)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 7)
    On Out_9

    Jump Pallet(1, 11)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 22)
    On Out_9
    
    Jump Pallet(1, 7)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 4)
    On Out_9
    
    Jump Pallet(1, 22)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 13)
    On Out_9
	
	Jump Pallet(1, 4)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 15)
    On Out_9
    
    Jump Pallet(1, 13)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 16)
    On Out_9
    
	Jump Pallet(1, 15)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 24)
    On Out_9
    
    Jump Pallet(1, 16)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 27)
    On Out_9
    
    Jump Pallet(1, 24)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 17)
    On Out_9
    
    Jump Pallet(1, 27)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 24)
    On Out_9
    
    Jump Pallet(1, 17)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 26)
    On Out_9
    
    Jump Pallet(1, 24)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 15)
    On Out_9
    
    Jump Pallet(1, 26)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 23)
    On Out_9
    
    Jump Pallet(1, 15)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 4)
    On Out_9
    
    Jump Pallet(1, 23)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 30)
    On Out_9
    
    Jump Pallet(1, 4)
    Off Out_9
    Wait 0.5
    Jump Pallet(1, 7)
    On Out_9
    
    Home
    Motor Off
Fend

```
## Diagrama de flujo de la rutina de movimiento de huevos con patrón de caballo de ajedrez

![image](https://github.com/jotavo21/Lab03_Robotica_2025_2/blob/main/Diagrama%20de%20Flujo.png)

## Simulación e Implementación de trayectoria en manipulador EPSON T3-401S

### Simulación en EPSON RC+ 7.5.2
[![Video 1](https://img.youtube.com/vi/yv0lNhRmCGE/0.jpg)](https://www.youtube.com/watch?v=yv0lNhRmCGE)

### Implementación en manipulador EPSON T3-401S

[![Video 2](https://img.youtube.com/vi/Mw_c045Ki5M/0.jpg)](https://www.youtube.com/watch?v=Mw_c045Ki5M)
