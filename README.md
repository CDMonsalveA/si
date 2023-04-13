# si
from pulp import *

demanda = [100, 50]
Utilidad = [45,60]

tiempos = [[15,10],[15,30],[15,5],[10,5]]

# i = A,B
# j = 1,2,3,4
# Xi = cantidad de producto i en maquina j
# Funcion objetivo
# maximizar 45*XA + 60*XB
# Restricciones
# Xi <= Demanda
# sum(Xi*Tiempo ij) <= 2400 for all j
# Xi >= 0

# create pulp problem
prob = LpProblem("Problema de la fabrica", LpMaximize)
# Creatre variables
X = LpVariable.dicts("X", [i for i in range(len(demanda))], lowBound=0, cat='Integer')
# Objective function
prob += lpSum([Utilidad[i]*X[i] for i in range(len(demanda))])
# Constraints
for i in range(len(demanda)):
    prob += X[i] <= demanda[i]
for j in range(4):
    prob += lpSum([X[i]*tiempos[j][i] for i in range(len(demanda))]) <= 2400
# Solve
prob.solve()
# Print solution
print("Status:", LpStatus[prob.status])
for v in prob.variables():
    print(v.name, "=", v.varValue)
    # print the objective function value
print("Total Utilidad = ", value(prob.objective))
print(prob)
