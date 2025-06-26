import networkx as nx
import matplotlib.pyplot as plt

# Define the satellite and sensor graphs
satellite_graph = nx.DiGraph()
sensor_graph = nx.DiGraph()

# Satellite Graph: 2 nodes
satellite_graph.add_edges_from([
    ("Satellite_1", "Satellite_2")
])

# Sensor Graph: 3 nodes in a cycle
sensor_graph.add_edges_from([
    ("Sensor_1", "Sensor_2"),
    ("Sensor_2", "Sensor_3"),
    ("Sensor_3", "Sensor_1")
])

# Create the conormal product graph
combined_graph = nx.DiGraph()

# Add combined nodes
for s_node in satellite_graph.nodes:
    for i_node in sensor_graph.nodes:
        combined_graph.add_node(f"{s_node}-{i_node}")

# Add edges based on satellite graph
for (u1, v1) in satellite_graph.edges:
    for i_node in sensor_graph.nodes:
        combined_graph.add_edge(f"{u1}-{i_node}", f"{v1}-{i_node}")

# Add edges based on sensor graph
for (u2, v2) in sensor_graph.edges:
    for s_node in satellite_graph.nodes:
        combined_graph.add_edge(f"{s_node}-{u2}", f"{s_node}-{v2}")

# Dominating nodes to highlight
dominating_nodes = [
    "Satellite_1-Sensor_1",
    "Satellite_2-Sensor_3",
    "Satellite_1-Sensor_2"
]

# Draw the conormal product graph
plt.figure(figsize=(6, 4))
pos = nx.spring_layout(combined_graph, seed=40)

nx.draw(combined_graph, pos, with_labels=True, node_size=800,
        node_color='lightblue', font_size=9, edge_color='gray')

# Highlight dominating nodes
nx.draw_networkx_nodes(combined_graph, pos,
                       nodelist=dominating_nodes, node_color='orange')

plt.title("Conormal Product of Satellite and Sensor Networks\nHighlighted Dominating Nodes", fontsize=14)
plt.axis('off')
plt.show()

