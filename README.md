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


## Niveles de velocidad para movimientos manuales

En el software EPSON RC+ 7.0+, los movimientos manuales del robot se realizan utilizando dos niveles de velocidad predefinidos: Low y High. Estos niveles determinan la rapidez con la que se desplaza el robot cuando se mueve mediante las herramientas presentes en la pestaña de configuración del robot, lo que hace las veces de teach pendant. El nivel Low corresponde a una velocidad reducida, adecuada para aproximaciones finas y trabajos en zonas cercanas a obstáculos o al propio operador, mientras que el nivel High permite movimientos más rápidos, útiles cuando el robot se encuentra lejos de elementos de riesgo. Aun cuando se seleccione el nivel alto, los movimientos manuales siguen ejecutándose en modo de potencia baja.

Para cambiar entre los niveles Low y High, se debe ingresar al Robot Manager (presionando F6), luego se selecciona la pestaña Mover y enseñar. En esta ventana se encuentra la subsección denominada Mover, dentro del cual aparece el campo Velocidad. Este campo contiene un menú desplegable en el que se puede seleccionar directamente el nivel deseado. El ajuste elegido se aplica inmediatamente a todos los desplazamientos manuales que se realicen desde esa misma interfaz, incluyendo los movimientos continuos y los movimientos paso a paso asociados a la opción distancia de movimiento.



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

