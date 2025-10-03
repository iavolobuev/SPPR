from scipy.optimize import linprog
import numpy as np

supply = [200, 100, 200, 400, 400]
demand = [200, 400, 100, 200, 100]

costs = [
    [1, 7, 12, 2, 5],
    [2, 3, 8, 4, 7],
    [3, 5, 4, 6, 9],
    [4, 4, 3, 8, 2],
    [5, 3, 7, 10, 1]
]

c = np.array(costs).flatten()

a_ub = []
b_ub = []

for i in range(5):
    row = [0] * 25
    for j in range(5):
        row[i * 5 + j] = 1
    a_ub.append(row)
    b_ub.append(supply[i])

a_eq = []
b_eq = []

for j in range(5):
    row = [0] * 25
    for i in range(5):
        row[i * 5 + j] = 1
    a_eq.append(row)
    b_eq.append(demand[j])

result = linprog(c, A_ub=a_ub, b_ub=b_ub, A_eq=a_eq, b_eq=b_eq, method='highs')

print("Минимальная стоимость:", result.fun)

print("Матрица перевозок:")
x_optimal = result.x.reshape(5, 5)
print(x_optimal.round().astype(int))
