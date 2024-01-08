## Práctica voluntaria XP

+ Pablo Pajón Area
p.pajon@udc.es
+ Ánder Sánchez Penas
ander.sanchez.penas@udc.es
+ Heitor Cambre Garcia
heitor.cambre@udc.es

## Ejercicio 1
*Tienes 5000 euros disponibles para invertirlos durante los próximos cuatro años. Al inicio de cada año puedes invertir parte del dinero en depósitos a un año o a dos años. Los depósitos a un año pagan un interés del 3%, mientras que los depósitos a dos años pagan un 7% al final de los dos años. El objetivo es conseguir que al cabo de los tres años tu capital sea lo mayor posible.*
+ Variable de decisión:
$x_{ij}$ ($i$ es el año y $j$ el tipo de inversion)

+ Función objetivo:
$$\text{Maximizar } Z = x_{50}$$
+ Sujeto a:
$$x_{ij} \geq 0$$ $$x_{10} + x_{11} + x_{12} = 5000$$
$$x_{10} + 1.03x_{11} = x_{20} + x_{21} + x_{22}$$
$$x_{20} + 1.03x_{21} + 1.07x_{12} = x_{30} + x_{31} + x_{32}$$
$$x_{30} + 1.03x_{31} + 1.07x_{22} = x_{40} + x_{41} + x_{42}$$
$$x_{40} + 1.03x_{41} + 1.07x_{32} = x_{50}$$

+ Resolución (con LPSolve IDE):
```
/* Objective function */
max: x50;

/* Variable bounds */
x10 >= 0; x11 >= 0; x12 >= 0; x20 >= 0; x21 >= 0; x22 >= 0; x30 >= 0; x31 >= 0; x32 >= 0; x40 >= 0; x41 >= 0; x42 >= 0; x50>=0;
x10+x11+x12=5000;
x10+1.3*x11=x20+x21+x22;
x20+1.3*x21+1.7*x12=x30+x31+x32;
x30+1.3*x31+1.7*x22=x40+x41+x42;
x40+1.3*x41+1.7*x32=x50;
```

+ Resultados obtenidos:
![[resultados p1 xp.png]]
Para maximizar el capital que tendremos dentro de 3 años:
+ Este año invertimos en depósitos a 1 año todo nuestro dinero. 1500\*1.04=1560€.
+ El segundo año invertimos también en depósitos a 1 año nuestro dinero. 1560*\1.04=1622.4€.
+ El tercer año invertimos también en depósitos a 1 año nuestro dinero. 1622.4€\*1.04=1687.296€.

## Ejercicio 2
*Una empresa produce listones de madera en cuatro medidas: pequeño, mediano,
grande y extra grande. Estos listones pueden producirse en tres máquinas: A, B y C. La
cantidad de metros que puede producir por hora cada máquina es:*

|    | A | B | C |
|----|----|----|----|
| Pequeño | 350 | 600 | 850 |
| Mediano | 100 | 500 | 750 |
| Grande | 175 | 400 | 350 |
| Extra grande | 140 | 100 | 500 |

*Supongamos que cada máquina puede ser usada 80 horas semanales y que el coste
operativo por hora de cada una es 70, 50 y 40 u.m. respectivamente. Si se
necesitan 12000, 9000, 6000 y 7000 metros de cada tipo de listones por semana,
formular un modelo para minimizar costes.*

+ Variable de decisión:
$x_{ij}$

$i$ representa el avión, $j$ representa la aldea. Por ejemplo, $x_{11}$ sería la cantidad de viajes que hace el avión 1 a la aldea 1.

+ Función objetivo:
$$\text{Maximizar } Z = 10x_{11} + 8x_{12} + 6x_{13} + 9x_{14} + 12x_{15}$$$$   + 5x_{21} + 3x_{22} + 8x_{23} + 4x_{24} + 10x_{25}$$ $$  + 7x_{31} + 9x_{32} + 6x_{33} + 10x_{34} + 4x_{35}$$

+ Sujeto a:
$$x_{ij}\geq0$$
$$x_{11} + x_{12} + x_{13} + x_{14} + x_{15} \leq 50$$
$$x_{21} + x_{22} + x_{23} + x_{24} + x_{25} \leq 90$$
$$x_{31} + x_{32} + x_{33} + x_{34} + x_{35} \leq 60$$
$$x_{11} + x_{21} + x_{31} \leq 100$$
$$x_{12} + x_{22} + x_{32} \leq 80$$
$$x_{13} + x_{23} + x_{33} \leq 70$$
$$x_{14} + x_{24} + x_{34} \leq 40$$
$$x_{15} + x_{25} + x_{35} \leq 20$$

+ Resolución (con LPSolve IDE):
```
/* Objective function */
max:  10x11 + 8x12 + 6x13 + 9x14 + 12x15 + 5x21 + 3x22 + 8x23 + 4x24 + 10x25 + 7x31 + 9x32 + 6x33 + 10x34 + 4x35;

/* Variable bounds */

x11 + x12 + x13 + x14 + x15 <= 50;
x21 + x22 + x23 + x24 + x25 <= 90;
x31 + x32 + x33 + x34 + x35 <= 60;
x11 + x21 + x31 <= 100;
x12 + x22 + x32 <= 80;
x13 + x23 + x33 <= 70;
x14 + x24 + x34 <=  40;
x15 + x25 + x35 <= 20;
int x11, x12, x13, x14, x15, x21, x22, x23, x24, x25, x31, x32, x33, x34, x35;
```

+ Resultados obtenidos:
![[XP programación lineal.png]]
Para maximizar la cantidad de alimento se desprecia que haya aldeas que se queden sin alimento:
+ El avión 1 solo viaja a la aldea 1, pero un total de 50 veces, llevando 10 U de alimento de cada vez. Esto nos da 500 U de alimento.
+ El avión 2 hace 90 viajes, 70 a la aldea 3 y 20 a la aldea 5. Esto nos da 560+200=760 U de alimento.
+ El avión hace 60 viajes, 20 a la aldea 2 y 40 a la aldea 4. Esto nos da 180+400=580 U de alimento.

En total, los 3 aviones han llevado 500+580+760=1840 U de alimento.
