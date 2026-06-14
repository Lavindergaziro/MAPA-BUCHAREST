# MAPA-BUCHAREST
MODELO DO MAPA DA ROMENIA EM PYTHON.
# C:\Users\User\Documents\MAPA_ROMENIA
# 
# 1. Modelando o mapa da Romênia como um grafo (Cidades e as distâncias entre elas)
mapa_romenia= {'Arad': [('Zerind', 75), ('Sibiu', 140), ('Timisoara', 118)],
    'Zerind': [('Arad', 75), ('Oradea', 71)],
    'Oradea': [('Zerind', 71), ('Sibiu', 151)],
    'Sibiu': [('Arad', 140), ('Oradea', 151), ('Fagaras', 99), ('Rimnicu Vilcea', 80)],
    'Timisoara': [('Arad', 118), ('Lugoj', 111)],
    'Lugoj': [('Timisoara', 111), ('Mehadia', 70)],
    'Mehadia': [('Lugoj', 70), ('Drobeta', 75)],
    'Drobeta': [('Mehadia', 75), ('Craiova', 120)],
    'Craiova': [('Drobeta', 120), ('Rimnicu Vilcea', 146), ('Pitesti', 138)],
    'Rimnicu Vilcea': [('Sibiu', 80), ('Craiova', 146), ('Pitesti', 97)],
    'Fagaras': [('Sibiu', 99), ('Bucharest', 211)],
    'Pitesti': [('Rimnicu Vilcea', 97), ('Craiova', 138), ('Bucharest', 101)],
    'Bucharest': [('Fagaras', 211), ('Pitesti', 101), ('Giurgiu', 90), ('Urziceni', 85)],
    'Giurgiu': [('Bucharest', 90)],
    'Urziceni': [('Bucharest', 85), ('Vaslui', 142), ('Hirsova', 98)],
    'Hirsova': [('Urziceni', 98), ('Eforie', 86)],
    'Eforie': [('Hirsova', 86)],
    'Vaslui': [('Urziceni', 142), ('Iasi', 92)],
    'Iasi': [('Vaslui', 92), ('Neamt', 87)],
    'Neamt': [('Iasi', 87)]}

def dijkstra(grafo, inicio, destino):
    # Fila de prioridade guarda tuplas: (custo_total, cidade_atual, caminho_ate_aqui)
    fila_prioridade = [(0, inicio, [inicio])]
    visitados = set()

    while fila_prioridade:
        (custo, cidade_atual, caminho) = heapq.heappop(fila_prioridade)

        # Se já visitamos essa cidade com um custo menor, ignora
        if cidade_atual in visitados:
            continue

        visitados.add(cidade_atual)

        # Se chegamos em Bucareste, retorna o resultado
        if cidade_atual == destino:
            return custo, caminho

        # Explora as cidades vizinhas
        for (vizinho, distancia) in grafo.get(cidade_atual, []):
            if vizinho not in visitados:
                heapq.heappush(fila_prioridade, (custo + distancia, vizinho, caminho + [vizinho]))

    return float("inf"), []

# --- Execução do Código ---
cidade_partida = 'Arad'  # Você pode mudar para 'Timisoara', 'Oradea', etc.
destino_final = 'Bucharest'

custo_total, rota = dijkstra(mapa_romenia, cidade_partida, destino_final)

print(f"--- ROTA OTIMIZADA PARA BUCARESTE ---")
print(f"Partindo de: {cidade_partida}")
print(f"Caminho a seguir: {' -> '.join(rota)}")
print(f"Distância total: {custo_total} km")
