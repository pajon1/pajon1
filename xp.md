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

+ Resolución (con Pyomo):
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

model.con1 = Constraint(expr=model.x10 + model.x11 + model.x12 
                        == 5000)

model.con2 = Constraint(expr=model.x10 + 1.03 * model.x11 
                        == model.x20 + model.x21 + model.x22)

model.con3 = Constraint(expr=model.x20 + 1.03 * model.x21 + 1.07 * model.x12 
                        == model.x30 + model.x31 + model.x32)

model.con4 = Constraint(expr=model.x30 + 1.03 * model.x31 + 1.07 * model.x22 
                        == model.x40 + model.x41 + model.x42)

model.con5 = Constraint(expr=model.x40 + 1.03 * model.x41 + 1.07 * model.x32 
                        == model.x50)

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

$i$ representa la maquina con la misma letra.
$j$ se utliza para diferenciar las hora de lkos tipos de maderas cuando es necesario.

+ Función objetivo:
$$\text{Minimizar } Z =70x_{a} + 50x_{b} + 40x_{c}$$

+ Sujeto a:
$$x_{i} \leq 80$$
$$350x_{ap} + 600x_{bp} + 800x_{cp} = 12000$$
$$100x_{am} + 500x_{bm} + 750x_{cm} = 9000$$
$$175x_{ag} + 400x_{bg} + 350x_{cg} = 6000$$
$$140x_{ax} + 100x_{bx} + 500x_{cx} = 7000$$
$$x_{ap} + x_{am} + x_{ag} + x_{ax} = x_{a}$$
$$x_{bp} + x_{bm} + x_{bg} + x_{bx} = x_{b}$$
$$x_{cp} + x_{cm} + x_{cg} + x_{cx} = x_{c}$$

+ Resolución (con Pyomo):
```
from pyomo.environ import *
from pyomo.opt import SolverFactory


model = ConcreteModel()
model.dual = Suffix(direction=Suffix.IMPORT)

model.xPA = Var(within=NonNegativeReals)
model.xPB = Var(within=NonNegativeReals)
model.xPC = Var(within=NonNegativeReals)
model.xMA = Var(within=NonNegativeReals)
model.xMB = Var(within=NonNegativeReals)
model.xMC = Var(within=NonNegativeReals)
model.xGA = Var(within=NonNegativeReals)
model.xGB = Var(within=NonNegativeReals)
model.xGC = Var(within=NonNegativeReals)
model.xEA = Var(within=NonNegativeReals)
model.xEB = Var(within=NonNegativeReals)
model.xEC = Var(within=NonNegativeReals)

model.obj = Objective(expr=70(model.xPA + model.xMA + model.xGA + model.xEA) 
                      + 50(model.xPB + model.xMB + model.xGB + model.xEB) 
                      + 40(model.xPC + model.xMC + model.xGC + model.xEC) , sense=minimize)


model.con1 = Constraint(expr=350model.xPA + 600model.xPB + 850model.xPC == 12000)
model.con2 = Constraint(expr=100model.xMA + 500model.xMB + 750model.xMC == 9000)
model.con3 = Constraint(expr=175model.xGA + 400model.xGB + 350model.xGC == 6000)
model.con4 = Constraint(expr=140model.xEA + 100model.xEB + 500*model.xEC == 7000)

model.con5 = Constraint(expr=model.xPA + model.xMA + model.xGA + model.xEA <= 80)
model.con6 = Constraint(expr=model.xPB + model.xMB + model.xGB + model.xEB <= 80)
model.con7 = Constraint(expr=model.xPC + model.xMC + model.xGC + model.xEC <= 80)

results = SolverFactory('glpk').solve(model)

print("Valor óptimo de la función objetivo:", model.obj())
for v in model.component_data_objects(Var, active=True):
    print(f"{v}: {value(v)}")
```

+ Resultados obtenidos:
```
Valor óptimo de la función objetivo: 2325.71429
x1:0
x2:0
x3:58.14286
x4:0
x5:0
x6:0
x7:0
x8:0
x9:0
x10:0
x11:0
x12:15
x13:12
x14:17.14286
x15:0
```
+ El coste óptimo es aproximadamente de 2290.42€
+ Se emplea la máquina C para producir madera pequeña durante 14 horas y 7 minutos (aprox.)
+ Se emplea la máquina C para producir madera mediana durante 12 horas.
+ Se emplea la máquina C para producir madera grande durante 17 horas y 9 minutos (aprox.)
+ Se emplea la máquina C para producir madera extra grande durante 14 horas.
