from collections import deque

def create_graph(nodes):
    graph = {}
    h = {}
    i = 0
    while i<nodes:
        node = input("Enter the node value: ").upper()
        edges = input("Enter the edges of the node: ").split(",")
        for j in range(len(edges)):
            edges[j] = edges[j].upper()
        h_value = int(input("Enter the heuristic value of node: "))
       
        graph[node] = edges
        h[node] = h_value
        i=i+1
       
    return graph,h

def heuristic(node,dq,h):
    val = h[node[0]]

    if len(dq)== 0:
        dq.append(node)
    else:
        for i in range(0,len(dq)):
            if h[dq[i][0]] > val:
                dq.insert(i,node)
                break
        else:
            dq.append(node)

    return dq

def cost_calc(path,h,start,goal):
    final_path = []
    cost = 0
    current = goal
    while current != None:
        print(f"{current}\n{final_path}\n")
        final_path.append(current)
        for item in path:
            if current == item[0]:
                current = item[1]

    final_path.reverse()

    for item in final_path:
        if item == start or item == goal:
            continue
        else:
            cost = cost+h[item]

    return final_path,cost

def greedy(start,goal,graph,h):
    open = deque([[start,None]])
    closed = set()
    path = []

    while open:
        print(f"Open List: {open}\nPath: {path}\n")
        node = open.popleft()

        if node[0] == goal:
            closed.add(node[0])
            path.append(node)
            print(f"Open List: {open}\nPath: {path}\n")
            final,cost = cost_calc(path,h,start,goal)
            return final,cost

        closed.add(node[0])
        path.append(node)

        for neigbour in graph[node[0]]:
            if neigbour in closed or neigbour == '':
                continue
            elif neigbour not in open:
                temp = []
                temp.append(neigbour)
                temp.append(node[0])
                open = heuristic(temp,open,h)

    print("Failure(No Path exisits to goal)")
    return None,None

def main():
    print("1. Perform Greedy Best First Search\n2. Re-Enter the Graph\n3. Exit")
    nodes = int(input("Enter the no. of nodes: "))
    g1,h = create_graph(nodes)
    print(f"\nGraph: {g1}\n")
    print(f"Heuristic Values: {h}\n")

    while True:
        print("1. Perform Greedy Best First Search\n2. Re-Enter the Graph\n3. Exit")
        c = input("Enter your Choice: ")

        match c:
            case "1":
                print(f"\nGraph: {g1}\n")
                print(f"Heusistic Values: {h}\n")
                start = input("Enter start node: ").upper()
                goal = input("Enter goal node: ").upper()        
                ls,cost = greedy(start,goal,g1,h)
                if ls == None and cost == None:
                    continue
                else:
                    print(f"Final Path: {ls}\nHeuristic Cost: {cost}\n")

            case "2":
                nodes = int(input("Enter the no. of nodes: "))
                g1,h = create_graph(nodes)
                print(f"\nGraph: {g1}\n")
                print(f"Heusistic Values: {h}\n")

            case "3":
                print("Exiting Program...")
                break


if __name__ == "__main__":
    main()