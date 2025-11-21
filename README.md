# Laboratorio 03 Robótica - Robótica Industrial - Análisis y Operación del Manipulador EPSON T3-401S

## Presentado por Juan Esteban Otavo García y Ian Saonni Rodríguez Pulido

En este laboratorio se estudiaron las funciones básicas del manipulador industrial EPSON T3-401S utilizando el software EPSON RC 7.0+. Para ello, se programó una rutina que permitiera al manipulador mover dos huevos de forma intercalada, replicando el movimiento del caballo de ajedrez dentro de una matriz de posiciones de 5×6 (una cubeta de huevos común). El objetivo fue que cada uno de los huevos recorriera las 30 posiciones de la cubeta.

## Cuadro comparativo entre manipuladores

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
| **Tipo de montaje** | Piso, pared o invertido | Piso, pared, invertido o en ángulo | Piso o mesa |
| **Controlador** | DX100 / DX200 | IRC5 Compact | EPSON RC+ 7.0 / Controlador integrado |
| **Fuente de alimentación** | 200–230 V AC trifásico | 200–600 V AC trifásico | 100–240 V AC monofásico |
| **Grado de protección** | IP54 (cuerpo) / IP67 (muñeca) | IP54 (cuerpo) / IP67 (muñeca) | IP20 |
| **Rango de temperatura de operación** | 0–45 °C | 5–45 °C | 5–40 °C |
| **Flange (montaje de herramienta)** | ISO 9409-1-A50 | ISO 9409-1-A50 | ISO 9409-1-31.5 |
| **Aplicaciones típicas** | Manipulación general, soldadura, carga/descarga, ensamblaje, paletizado | Ensamblaje de precisión, manipulación, laboratorio, atornillado | Pick & place, electrónica, empaques, laboratorio |
| **Ventajas destacadas** | Gran alcance y estructura robusta | Alta precisión y tamaño compacto | Ligero, rápido, eficiente y fácil de instalar |
| **Limitaciones** | Repetibilidad menor que IRB 140 | Alcance corto | Carga y alcance reducidos; no para trabajos pesados |
