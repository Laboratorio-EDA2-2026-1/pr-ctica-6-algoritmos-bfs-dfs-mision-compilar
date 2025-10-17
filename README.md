[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/xkhqJnty)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21013835&assignment_repo_type=AssignmentRepo)
# Práctica: Búsqueda en Profundidad (DFS)

## Problema de las N Reinas

El **problema de las N reinas** consiste en colocar `N` reinas en un tablero de ajedrez de `N × N` de manera que **ninguna reina pueda atacar a otra**.  
Es decir, no puede haber dos reinas en la misma fila, columna o diagonal.

El uso de **DFS** para resolver el problema de las `N` reinas consiste en colocar una reina en una posición y luego explorar posiciones consecutivas que sean válidas según la definición del problema.

---

### Planteamiento de los ejercicios

En esta práctica se resolverán **tres ejercicios**, dos relacionados con el algoritmo DFS y uno relacionado con el algoritmo BFS:

---

#### Ejercicio 1

Definir una función en **Python** que reciba como entrada un entero `N` (`1 ≤ N ≤ 8`) correspondiente al tamaño del tablero.

La salida de la función es un **arreglo `P`** de tamaño `N` con enteros menores que `N` indicando una posible posición de las `N` reinas que soluciona el problema.

Si `i` es un valor menor a `N`, entonces el valor `P[i]` indica en qué **renglón de la i-ésima columna** del tablero colocar una reina para obtener una solución.

**Referencia:** además del pseudocódigo visto en clase, pueden revisar el siguiente enlace:  
[Visualización del problema de las N reinas (USFCA)](https://www.cs.usfca.edu/~galles/visualization/RecQueens.html)

---

##### Ejemplo

Supongamos que la función se llama `nreinas`, y la entrada de la función es `N = 4`.  
Una posible salida es:

`P = [1, 3, 0, 2]`

Esto corresponde al siguiente tablero:

|   |   | Q |   |
|---|---|---|---|
| Q |   |   |   |
|   |   |   | Q |
|   | Q |   |   |

Por lo tanto, el resultado de `nreinas(4)` es:

`[1, 3, 0, 2]`

---

##### SOLUCION PROBLEMA 1

from typing import List

def en_conflicto(parcial: List[int], fila: int, col: int) -> bool:
    
    for c, r in enumerate(parcial):
        if r == fila:
            return True
        if abs(r - fila) == abs(c - col):
            return True
    return False


def nreinas(N: int) -> List[List[int]]:
    
    soluciones: List[List[int]] = []
    parcial: List[int] = []

    def dfs(col: int):
        if col == N:
            soluciones.append(parcial.copy())
            return

        for fila in range(N):
            if not en_conflicto(parcial, fila, col):
                parcial.append(fila)
                dfs(col + 1)
                parcial.pop()

    dfs(0)
    return soluciones


if __name__ == "__main__":
        N = int(input("Ingrese el tamaño de N (1 - 8): "))
        soluciones = nreinas(N)
        print(f"\nSe encontraron {len(soluciones)} soluciones para N={N}:\n")
        for i, sol in enumerate(soluciones, 1):
            print(f"Solución {i}: {sol}")
            

---

#### Ejercicio 2

Definir una función en **Python** que reciba **dos entradas**:

1. Un entero `N` (`1 ≤ N ≤ 8`) correspondiente al tamaño del tablero.  
2. Un arreglo `A` de enteros tal que `0 ≤ len(A) ≤ N`.  
   (Es decir, puede ser un arreglo vacío, y puede tener a lo más `N` enteros que indican **posiciones no disponibles**.)

La salida de la función es un **arreglo `P`** de tamaño `N` que indica posiciones posibles para las `N` reinas que solucionan el problema.

---

##### Ejemplo

Supongamos que nuestra función se llama `nreinas2`, y que recibe como entradas:

`N = 4`  
`A = [1, 3, 0, 2]`

Una posible salida es:

`[2, 0, 3, 1]`

correspondiente al siguiente tablero:

|   | Q | X |   |
|---|---|---|---|
| X |   |   | Q |
| Q |   |   | X |
|   | X | Q |   |

> Las **X** indican posiciones no disponibles.

Por lo tanto, el resultado de `nreinas2(4, A)` es:

`[2, 0, 3, 1]`

---

### Punto Extra

Para cada uno de los dos problemas de las `N` reinas, si además del resultado que pide cada inciso, la función devuelve **todas las soluciones posibles**, entonces se obtiene **un punto extra** en la **tarea** correspondiente a estos temas.

---

##### SOLUCION PROBLEMA 2

from typing import List, Optional

def en_conflicto(parcial: List[int], fila: int, col: int) -> bool:
    for c, r in enumerate(parcial):
        if r == fila:
            return True
        if abs(r - fila) == abs(c - col):
            return True
    return False


def nreinas_con_obstaculos(N: int, A: List[int]) -> List[List[int]]:
    soluciones: List[List[int]] = []
    parcial: List[int] = []

    def fila_bloqueada(col: int) -> Optional[int]:
        if 0 <= col < len(A):
            return A[col]
        return None

    def dfs(col: int):
        if col == N:
            soluciones.append(parcial.copy())
            return

        bloqueada = fila_bloqueada(col)
        for fila in range(N):
            if bloqueada is not None and fila == bloqueada:
                continue 
            if not en_conflicto(parcial, fila, col):
                parcial.append(fila)
                dfs(col + 1)
                parcial.pop()

    dfs(0)
    return soluciones

if __name__ == "__main__":
    N = int(input("Ingresa el tamaño del tablero (1 - 8): "))
    entrada = input("Ingresa el arreglo A (numero 1, numero 2, ... , numero N): ").strip()
    if entrada:
        A = [int(x) for x in entrada.split(",")]
    else:
        A = []

    soluciones = nreinas_con_obstaculos(N, A)
    print(f"\nSe encontraron {len(soluciones)} soluciones para N={N} con obstáculos {A}:\n")
    if soluciones:
        for i, sol in enumerate(soluciones, 1):
            print(f"Solución {i}: {sol}")
    else:
        print("No se encontraron soluciones posibles.")

---

## Problema del intervalo

Definir una función que reciba **dos datos de entrada**:

- Un arreglo `A` de enteros no negativos de tamaño `N > 0`.
- Un número racional no negativo `q`.

La salida producida por la función es **una lista con las dos posiciones** en el arreglo `A`, señalando el intervalo más pequeño que contiene al número `q`.

Para la definición de la función, dividir de manera **recursiva** el arreglo de entrada hasta obtener subarreglos con un solo dato (como en Merge Sort).

---

### Ejemplo

Supongamos que el número de entrada es:

`q = 8.13`  
`A = [4, 0, 7, 11, 9, 12, 56, 3]`

Para encontrar el intervalo más pequeño, la función divide el arreglo buscando:

- el entero que se aproxima **por abajo** de `q`, y  
- el que se aproxima **por arriba** de `q`.

El proceso interno puede visualizarse así:


```
[4, 0, 7, 11, 9, 12, 56, 3]
[4, 0, 7, 11]   [9, 12, 56, 3]
[4, 0] [7, 11]   [9, 12] [56, 3]
[4] [0] [7] [11]   [9] [12] [56] [3]
```



La función identifica que **7** y **9** son los enteros más cercanos a `q` por debajo y por arriba, respectivamente.

Por lo tanto, el resultado de la función con esas entradas es:

`[2, 4]`

donde los valores son las posiciones que ocupan el **7** y el **9** en el arreglo original.
---

#### SOLUCION PROBLEMA DEL INTERVALO

def intervalo(A, q):
    menor = None
    mayor = None
    indiceme = -1
    indicema = -1

    for i in range(len(A)):
        if A[i] <= q:
            if menor is None or A[i] > menor:
                menor = A[i]
                indiceme = i
        if A[i] >= q:
            if mayor is None or A[i] < mayor:
                mayor = A[i]
                indicema = i

    if indiceme == -1:
        menor = min(A)
        indiceme = A.index(menor)
    if indicema == -1:
        mayor = max(A)
        indicema = A.index(mayor)

    return [indiceme, indicema]

N = int(input("Ingrese el tamaño del arreglo N: "))
A = []

print("Ingrese los valores del arreglo A:")
for i in range(N):
    valor = int(input())
    A.append(valor)

q = float(input("Ingrese el valor racional q: "))

resultado = intervalo(A, q)
print(resultado)

---

# Parte II. Búsqueda en Anchura (BFS)

## Descripción del problema

Considera el siguiente **mapa de las ciudades de Rumania** (ver figura).  
Los valores que aparecen en las aristas corresponden al **tiempo promedio en minutos** que toma llegar desde un punto a otro con el que tenga conexión directa.  

Estos tiempos varían a lo largo del día, de acuerdo con el horario y las condiciones de tráfico.  
De manera específica, los tiempos de traslado se comportan así:

-  **De 00:00 a 5:59** → El tiempo de traslado es el que aparece en el mapa.  
-  **De 6:00 a 15:59** → El tiempo de traslado es **el doble** del que aparece en el mapa.  
  - Ejemplo: de **Bucharest a Giurgiu**, el tiempo de traslado es **180 minutos**.  
-  **De 16:00 a 23:59** → El tiempo de traslado entre cada dos puntos es **1.5 veces** el que aparece en el mapa.  
  - Ejemplo: de **Bucharest a Giurgiu**, el tiempo de traslado es **135 minutos**.

---

### Figura

**Figura 1.** Mapa de las ciudades de Rumania.  
*(Los valores en las aristas indican el tiempo promedio de traslado en minutos.)*

![Mapa de Rumania]((https://image1.slideserve.com/2592244/road-map-of-romania-l.jpg))

---

## Ejercicio

El objetivo de este ejercicio es **implementar un sistema** que pueda responder **cuál es el tiempo total de traslado** entre dos ciudades dadas, **considerando la hora de inicio del viaje**.

El cálculo debe **respetar los horarios** en los que cambian los tiempos estimados de traslado, de modo que si el viaje cruza un intervalo horario distinto, se debe ajustar el tiempo restante en consecuencia.

---

### Ejemplos

- **Ejemplo 1.**  
  Si el viaje es de **Giurgiu a Urziceni** y comienza a las **5:00 am**, entonces:
  - La primera parte del viaje (Giurgiu → Bucharest) dura **90 minutos**, porque inicia antes de las 6:00 am.  
  - Sin embargo, al llegar a Bucharest ya son **6:30 am**, y la segunda parte (Bucharest → Urziceni) ocurre después de las 6:00 am.  
    Por lo tanto, su duración se calcula con el **doble del tiempo** que aparece en el mapa, es decir **170 minutos**.

- **Ejemplo 2.**  
  Si el mismo viaje comienza a las **4:00 am**, ambos trayectos se calculan con el tiempo **original** del mapa, porque la llegada a Bucharest ocurre **antes de las 6:00 am**.

---

## Requisitos de implementación

- Utiliza el **algoritmo de Búsqueda en Anchura (BFS)** para recorrer el grafo de ciudades y determinar el camino más corto en número de aristas (no necesariamente en tiempo).
- Una vez encontrado el camino, **calcula el tiempo total del recorrido** ajustando los tiempos de cada tramo según el horario en que inicia.
- Se recomienda representar el grafo mediante un **diccionario de adyacencia** en Python.
- Considera representar el **horario de inicio** en formato de 24 horas (por ejemplo, `5.0` para 5:00 am o `13.5` para 13:30 pm).

---

### Sugerencias

- Puedes comenzar implementando una función `bfs_path(graph, start, goal)` que devuelva el camino más corto (lista de ciudades).
- Luego, define una función `travel_time(path, start_hour)` que calcule el tiempo total del recorrido considerando los horarios y ajustes de tráfico.
- Si lo deseas, imprime el recorrido y el tiempo acumulado entre cada ciudad intermedia.

---


#### SOLUCIÓN PROBLEMA PARTE 2 BFS TIEMPO TOTAL DE TRASLADO

from collections import deque
from typing import Dict, List, Optional

Ciudad = str
Grafo = Dict[Ciudad, Dict[Ciudad, int]]

def bfs_path(g: Grafo, origen: Ciudad, destino: Ciudad) -> Optional[List[Ciudad]]:
    if origen not in g or destino not in g:
        return None

    visitado = set([origen])
    pred: Dict[Ciudad, Optional[Ciudad]] = {origen: None}
    q = deque([origen])

    while q:
        u = q.popleft()
        if u == destino:
            break
        for v in g[u]:
            if v not in visitado:
                visitado.add(v)
                pred[v] = u
                q.append(v)

    if destino not in pred:
        return None

    camino: List[Ciudad] = []
    actual: Optional[Ciudad] = destino
    while actual is not None:
        camino.append(actual)
        actual = pred[actual]
    camino.reverse()
    return camino

def factor_horario(minutos: int) -> float:
    minutos=minutos % (24*60)
    if 0 <= minutos < 6*60:
        return 1.0
    if 6 * 60 <= minutos < 16*60:
        return 2.0
    return 1.5

def siguiente_cambio(minutos: int)->int:
    md=minutos % (24*60)
    dia_base= minutos - md
    bordes= [6*60, 16*60, 24*60]
    for b in bordes:
        if md < b:
            return dia_base + b
    return dia_base + 24*60

def tiempo_tramo_respetando_horario(inicio: int, duracion_base: int) -> int:
    tiempo_base_restante = duracion_base
    tiempo_real_acumulado = 0
    t_actual = inicio
    
    while tiempo_base_restante > 0:
        f = factor_horario(t_actual)
        cambio = siguiente_cambio(t_actual)
        tiempo_hasta_cambio = cambio - t_actual
        
        base_hasta_cambio = tiempo_hasta_cambio / f
        base_restante = tiempo_base_restante
        
        if base_hasta_cambio >= base_restante:
            tiempo_real_acumulado += base_restante * f
            tiempo_base_restante = 0
        else:
            tiempo_real_acumulado += tiempo_hasta_cambio
            tiempo_base_restante -= base_hasta_cambio
            t_actual = cambio
    
    return int(tiempo_real_acumulado)

def tiempo_total(g: Grafo, camino: List[Ciudad], inicio_min: int) -> int:
    total = 0
    t_actual = inicio_min
    
    print(f"\nRecorrido: {' → '.join(camino)}")
    hora_inicio = inicio_min // 60
    minuto_inicio = inicio_min % 60
    print(f"Hora de inicio: {hora_inicio:02d}:{minuto_inicio:02d}")
    print("\nDetalle del viaje:")
    print("-" * 60)
    
    for u, v in zip(camino, camino[1:]):
        base = g[u][v]
        real = tiempo_tramo_respetando_horario(t_actual, base)
        
        hora_inicio_seg = t_actual // 60
        minuto_inicio_seg = t_actual % 60
        t_fin = t_actual + real
        hora_fin_seg = t_fin // 60
        minuto_fin_seg = t_fin % 60
        
        print(f"{u} → {v}: {base} min base → {real} min real")
        print(f"  {hora_inicio_seg:02d}:{minuto_inicio_seg:02d} → {hora_fin_seg:02d}:{minuto_fin_seg:02d} | Acumulado: {total + real} min")
        
        total += real
        t_actual = t_fin
    
    print("-" * 60)
    print(f"Tiempo total: {total} minutos")
    return total

def grafo_rumania_demo() -> Grafo:
    g: Grafo = {
        "Giurgiu": {"Bucharest": 90},
        "Bucharest": {"Giurgiu": 90, "Urziceni": 85, "Pitesti": 101, "Fagaras": 211},
        "Urziceni": {"Bucharest": 85, "Hirsova": 98, "Vaslui": 142},
        "Hirsova": {"Urziceni": 98, "Eforie": 86},
        "Eforie": {"Hirsova": 86},
        "Vaslui": {"Urziceni": 142, "Iasi": 92},
        "Iasi": {"Vaslui": 92, "Neamt": 87},
        "Neamt": {"Iasi": 87},
        "Pitesti": {"Bucharest": 101, "Rimnicu Vilcea": 97, "Craiova": 138},
        "Rimnicu Vilcea": {"Pitesti": 97, "Craiova": 146, "Sibiu": 80},
        "Craiova": {"Pitesti": 138, "Rimnicu Vilcea": 146, "Drobeta": 120},
        "Drobeta": {"Craiova": 120, "Mehadia": 75},
        "Mehadia": {"Drobeta": 75, "Lugoj": 70},
        "Lugoj": {"Mehadia": 70, "Timisoara": 111},
        "Timisoara": {"Lugoj": 111, "Arad": 118},
        "Arad": {"Timisoara": 118, "Sibiu": 140, "Zerind": 75},
        "Zerind": {"Arad": 75, "Oradea": 71},
        "Oradea": {"Zerind": 71, "Sibiu": 151},
        "Sibiu": {"Arad": 140, "Oradea": 151, "Rimnicu Vilcea": 80, "Fagaras": 99},
        "Fagaras": {"Sibiu": 99, "Bucharest": 211}
    }
    return g

if __name__ == "__main__":
    g = grafo_rumania_demo()
    path = bfs_path(g, "Giurgiu", "Urziceni")
    print("Camino (BFS, aristas):", path)

    inicio_5am = 5 * 60
    if path:
        print("Tiempo total iniciando 5:00:", tiempo_total(g, path, inicio_5am), "min")

    inicio_4am = 4 * 60
    if path:
        print("Tiempo total iniciando 4:00:", tiempo_total(g, path, inicio_4am), "min")
