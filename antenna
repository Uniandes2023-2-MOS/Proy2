from pyomo.environ import *

def range_calculation(antenna, spot, width, range_var):
    x_s = spot % width
    y_s = spot // width
    res = 0
    for i in range(0, len(antenna)):
        x = i % width
        y = i // width
        local_range = range_var[antenna[i]]
        if (local_range > 0) and (local_range >= sqrt(abs((x - x_s)**2 + (y - y_s)**2))):
            res = 1
    return res


model = ConcreteModel()

indices = range(0,99)
width = 10

model.antenna = Var(indices, within=NonNegativeReals)
model.cover = Var(indices, within=NonNegativeReals)
model.cost = {0:0, 1:2, 2:5, 3:9}
model.range = {0:0, 1:3, 2:6, 3:10}
model.antenna_cost = Param(model.antenna, initialize=model.cost, default=0)

model.obj1 = Objective(expr=sum(model.antenna_cost[model.antenna[i]] for i in indices), sense=minimize)
model.obj2 = Objective(expr=sum(model.cover[i]  for i in indices), sense=maximize)

for i in indices:
    model.cover[i].value = range_calculation(model.antenna, i, width, model.range)

solver = SolverFactory('glpk')
solver.solve(model)

print("Objective Value 1 (Maximized):", model.obj1())
print("Objective Value 2 (Minimized):", model.obj2())
