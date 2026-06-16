# MAPA-BUCHAREST
MODELO DO MAPA DA ROMENIA EM PYTHON.

# 1. Modelando o mapa da Romênia como um grafo (Cidades e as distâncias entre elas)



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

# Grafo representando o mapa da Romênia (Capítulo 3 do Russell & Norvig)
# Formato: 'Cidade_Origem': [('Cidade_Vizinha', custo_real_da_estrada), ...]
mapa_romenia = {
    'Arad': [('Zerind', 75), ('Sibiu', 140), ('Timisoara', 118)],
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
    'Neamt': [('Iasi', 87)]
}
#🧠 **Explicação do Código – Algoritmo de Dijkstra aplicado ao mapa da Romênia**

Este código implementa o **algoritmo de Dijkstra**, um dos métodos mais conhecidos para encontrar o **caminho mais curto** entre dois pontos em um grafo.  
No contexto deste projeto, o grafo representa o **mapa da Romênia**, onde cada cidade é um **nó** e cada estrada entre cidades é uma **aresta** com um **peso** (a distância em quilômetros).

---

### 🗺️ **1. Modelagem do grafo**
O dicionário `mapa_romenia` define as conexões entre as cidades:
```python
'Arad': [('Zerind', 75), ('Sibiu', 140), ('Timisoara', 118)]
```
Cada chave é uma cidade, e o valor é uma lista de tuplas com as cidades vizinhas e suas respectivas distâncias.

---

### ⚙️ **2. Função `dijkstra()`**
A função recebe:
- `grafo`: o mapa da Romênia;  
- `inicio`: cidade de partida;  
- `destino`: cidade final.

Ela usa uma **fila de prioridade** (`heapq`) para sempre escolher o próximo nó com o **menor custo acumulado**.

**Etapas principais:**
1. Inicializa a fila com a cidade de partida e custo zero.  
2. Remove da fila a cidade com menor custo.  
3. Marca a cidade como visitada.  
4. Para cada vizinho, calcula o novo custo e adiciona à fila se ainda não foi visitado.  
5. Quando o destino é alcançado, retorna o custo total e o caminho percorrido.

---

### 🧭 **3. Execução**
```python
cidade_partida = 'Arad'
destino_final = 'Bucharest'
```
O algoritmo é executado e imprime:
```
--- ROTA OTIMIZADA PARA BUCARESTE ---
Partindo de: Arad
Caminho a seguir: Arad -> Sibiu -> Rimnicu Vilcea -> Pitesti -> Bucharest
Distância total: 418 km
```

---

### 📊 **4. Resultado**
O Dijkstra garante o **menor caminho possível** entre Arad e Bucareste, considerando todas as distâncias do grafo.  
Ele é amplamente usado em sistemas de **navegação, redes de transporte e roteamento de dados**.

---

💡 **Resumo conceitual:**
| Conceito | Explicação |
|-----------|------------|
| **Nó** | Cidade |
| **Aresta** | Estrada entre cidades |
| **Peso** | Distância em km |
| **Fila de prioridade** | Escolhe o menor custo acumulado |
| **Resultado** | Caminho mais curto até o destino |


# Heurística h(n): Distância em linha reta de cada cidade até BUCUBARESTE (Bucharest)
heuristica_bucareste = {
    'Arad': 366, 'Bucharest': 0, 'Craiova': 160, 'Drobeta': 242, 'Eforie': 161,
    'Fagaras': 176, 'Giurgiu': 77, 'Hirsova': 151, 'Iasi': 226, 'Lugoj': 244,
    'Mehadia': 241, 'Neamt': 234, 'Oradea': 380, 'Pitesti': 100, 'Rimnicu Vilcea': 193,
    'Sibiu': 253, 'Timisoara': 329, 'Urziceni': 80, 'Vaslui': 199, 'Zerind': 374
}

def busca_gulosa(inicio, objetivo, grafo, heuristica):
    # Cada elemento na fronteira será uma tupla: (valor_h, cidade_atual, caminho_ate_aqui)
    # Começamos com a cidade de origem
    fronteira = [(heuristica[inicio], inicio, [inicio])]
    
    # Conjunto para armazenar nós já expandidos (evita loops e redundâncias)
    visitados = set()
    
    print(f"--- Iniciando Busca Gulosa de {inicio} para {objetivo} ---")
    
    while fronteira:
        # Ordena a fronteira com base no valor da heurística h(n) -> Menor primeiro
        fronteira.sort(key=lambda x: x[0])
        
        # Remove o nó com menor h(n) da fronteira
        h_atual, cidade_atual, caminho = fronteira.pop(0)
        
        print(f"Expandindo: {cidade_atual} (h = {h_atual})")
        
        # Teste de Objetivo
        if cidade_atual == objetivo:
            return caminho
            
        # Se a cidade ainda não foi expandida
        if cidade_atual not in visitados:
            visitados.add(cidade_atual)
            
            # Varre os vizinhos da cidade atual
            for vizinho, _ in grafo.get(cidade_atual, []):
                if vizinho not in visitados:
                    novo_caminho = caminho + [vizinho]
                    # Insere na fronteira priorizando a heurística h(n) do vizinho
                    fronteira.append((heuristica[vizinho], vizinho, novo_caminho))
                    
    return None # Se a fronteira esvaziar e não achar o objetivo

# --- Execução do Teste Clássico do Livro (Saindo de Arad para Bucareste) ---
caminho_final = busca_gulosa('Arad', 'Bucharest', mapa_romenia, heuristica_bucareste)

print("\n--- Resultado ---")
if caminho_final:
    print("Caminho encontrado:", " -> ".join(caminho_final))
else:
    print("Não foi possível encontrar um caminho.")
    
custo_total, rota = dijkstra(mapa_romenia, cidade_partida, destino_final)

print(f"--- ROTA OTIMIZADA PARA BUCARESTE ---")
print(f"Partindo de: {cidade_partida}")
print(f"Caminho a seguir: {' -> '.join(rota)}")
print(f"Distância total: {custo_total} km")

#[AULAPYTHON.PY](https://github.com/user-attachments/files/28935784/AULAPYTHON.PY)


# IMAGEM DE CAMINHO DE BUCHAREST
#<img width="1536" height="1024" alt="Copilot_20260614_191838" src="https://github.com/user-attachments/assets/0c3db641-a195-460f-a607-db562278c87b" />

# IMAGEM 2
#<img width="1536" height="1024" alt="Copilot_20260614_192848" src="https://github.com/user-attachments/assets/7384e916-d0d7-4604-ae82-d796ce72e2b4" />

#🧠 **Explicação do funcionamento da Busca Gulosa.  

A **Busca Gulosa** é um algoritmo de busca heurística que tenta encontrar o caminho até o objetivo **escolhendo sempre o nó que parece mais promissor**, com base apenas na **heurística** — ou seja, uma estimativa da distância até o destino.  

---

### ⚙️ **Como ela funciona passo a passo**
1. **Início:**  
   O algoritmo começa na cidade inicial (por exemplo, *Arad*).  

2. **Heurística:**  
   Cada cidade tem um valor `h(n)` — a distância em linha reta até *Bucareste*. Esse valor indica o “quão perto” o nó parece estar do objetivo.  

3. **Expansão:**  
   A cada passo, o algoritmo escolhe **a cidade com o menor valor de heurística** entre as disponíveis.  
   Ele **não considera o custo real do caminho percorrido**, apenas o valor estimado até o destino.  

4. **Exploração:**  
   A cidade escolhida é expandida — ou seja, o algoritmo analisa suas vizinhas e adiciona essas cidades à fronteira (lista de possíveis próximos passos).  

5. **Critério de parada:**  
   Quando o algoritmo chega à cidade objetivo (*Bucharest*), ele encerra e retorna o caminho percorrido.  

---

### 📉 **Características principais**
| Aspecto | Descrição |
|----------|------------|
| **Base de decisão** | Apenas na heurística `h(n)` |
| **Vantagem** | Rápido e simples — ótimo para buscas aproximadas |
| **Desvantagem** | Pode escolher caminhos ruins, pois ignora o custo real (`g(n)`) |
| **Tipo de busca** | Informada (usa conhecimento adicional sobre o problema) |

---

### 🧭 **Exemplo prático**
No caso do mapa da Romênia:  
- Começando em **Arad (h=366)**, o algoritmo compara as cidades vizinhas: *Zerind (374)*, *Sibiu (253)* e *Timisoara (329)*.  
- Escolhe **Sibiu**, pois tem o menor valor de heurística.  
- Depois, de *Sibiu*, escolhe *Fagaras (176)*, e assim por diante, até chegar em *Bucharest (0)*.

---

Em resumo, a **Busca Gulosa** é como um viajante que sempre escolhe o caminho que *parece* mais curto olhando o mapa — mas nem sempre é o mais eficiente.  

# EXECUÇÃO DO ALGORITMO DIJKSTRA.
<img width="1536" height="1024" alt="Copilot_20260614_193625" src="https://github.com/user-attachments/assets/175811e7-5a43-4e94-84f6-368297b09be2" />

# 🧩 **Explicação para acompanhar a imagem da execução do código Dijkstra**

A imagem representa o resultado da execução do algoritmo **Dijkstra**, aplicado ao mapa da Romênia.  
Esse algoritmo é usado para encontrar o **caminho mais curto** entre duas cidades — neste caso, de **Arad** até **Bucareste**.

---

### ⚙️ **Como o Dijkstra funciona**
1. **Início:** O algoritmo parte da cidade inicial (*Arad*) e define o custo inicial como zero.  
2. **Exploração:** Ele analisa todas as cidades vizinhas e calcula o custo total para chegar a cada uma.  
3. **Escolha do menor custo:** A cada etapa, o algoritmo escolhe a cidade com o **menor custo acumulado** e continua o processo a partir dela.  
4. **Atualização dos custos:** Se encontrar um caminho mais curto para uma cidade já visitada, ele atualiza o custo.  
5. **Finalização:** Quando chega ao destino (*Bucareste*), o algoritmo retorna o **caminho otimizado** e a **distância total** percorrida.

---

### 🧭 **Resultado mostrado na imagem**
- **Partida:** Arad  
- **Caminho encontrado:** Arad → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest  
- **Distância total:** 418 km  

Esse é o **trajeto mais eficiente** segundo o cálculo das distâncias entre as cidades, demonstrando o poder do Dijkstra em resolver problemas de rotas e otimização de caminhos.  
## Busca A* no Mapa da Romênia

Este projeto implementa o algoritmo A* para encontrar o caminho mais curto entre cidades da Romênia, baseado no exemplo clássico do livro *Artificial Intelligence: A Modern Approach*.

## Como funciona
- Cada cidade é um nó.
- Estradas são arestas com pesos (distâncias).
- A função de avaliação é:
  f(n) = g(n) + h(n)

## Exemplo
Arad → Bucareste
Resultado: Caminho ótimo encontrado pelo A*.

## Aplicações
- GPS
- Jogos
- Robótica
- Inteligência Artificial


