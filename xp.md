## Práctica voluntaria XP

+ Pablo Pajón Area
p.pajon@udc.es
+ Ánder Sánchez Penas
ander.sanchez.penas@udc.es
+ Heitor Cambre Garcia
heitor.cambre@udc.es

## Ejercicio 1
*Tienes 5000 euros disponibles para invertirlos durante los próximos cuatro años.
Al inicio de cada año puedes invertir parte del dinero en depósitos a un año o a dos
años. Los depósitos a un año pagan un interés del 3 %, mientras que los depósitos a
dos años pagan un 7% al final de los dos años. El objetivo es conseguir que al cabo de
los cuatro años tu capital sea lo mayor posible.*
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
from pyomo.environ import *
from pyomo.opt import SolverFactory


model = ConcreteModel()
model.dual = Suffix(direction=Suffix.IMPORT)

model.x10 = Var(within=NonNegativeReals)
model.x11 = Var(within=NonNegativeReals)
model.x12 = Var(within=NonNegativeReals)
model.x20 = Var(within=NonNegativeReals)
model.x21 = Var(within=NonNegativeReals)
model.x22 = Var(within=NonNegativeReals)
model.x30 = Var(within=NonNegativeReals)
model.x31 = Var(within=NonNegativeReals)
model.x32 = Var(within=NonNegativeReals)
model.x40 = Var(within=NonNegativeReals)
model.x41 = Var(within=NonNegativeReals)
model.x42 = Var(within=NonNegativeReals)
model.x50 = Var(within=NonNegativeReals)

model.obj = Objective(expr=model.x50, sense=maximize)

model.con1 = Constraint(expr=model.x10 + model.x11 + model.x12 == 5000)
model.con2 = Constraint(expr=model.x10 + 1.03 * model.x11 == model.x20 + model.x21 + model.x22)
model.con3 = Constraint(expr=model.x20 + 1.03 * model.x21 + 1.07 * model.x12 == model.x30 + model.x31 + model.x32)
model.con4 = Constraint(expr=model.x30 + 1.03 * model.x31 + 1.07 * model.x22 == model.x40 + model.x41 + model.x42)
model.con5 = Constraint(expr=model.x40 + 1.03 * model.x41 + 1.07 * model.x32 == model.x50)

results = SolverFactory('glpk').solve(model)

print("Valor óptimo de la función objetivo:", model.obj())

for v in model.component_data_objects(Var, active=True):
    print(f"{v}: {value(v)}")
```

+ Resultados obtenidos:
```
Valor óptimo de la función objetivo: 5724.5
x10: 0.0
x11: 0.0
x12: 5000.0
x20: 0.0
x21: 0.0
x22: 0.0
x30: 0.0
x31: 0.0
x32: 5350.0
x40: 0.0
x41: 0.0
x42: 0.0
x50: 5724.5 
```
+ En el primer y tercer año invertimos todo al 7% de interés en 2 años teniendo a final 5724.5 

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
$$\text{Minimizar } Z =70x_{a} + 50x_{b} + 40x_{c}$$

+ Sujeto a:
$$x_{i} \geq 0$$
$$350x_{a} + 600x_{b} + 800x_{c} = 12000$$
$$100x_{a} + 500x_{b} + 750x_{c} = 9000$$
$$175x_{a} + 400x_{b} + 350x_{c} = 6000$$
$$140x_{a} + 100x_{b} + 500x_{c} = 7000$$

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
