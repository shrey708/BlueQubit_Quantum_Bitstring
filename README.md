# BlueQubit_Quantum_Bitstring
As a part of the BlueQubit Hackathon, I optimized and ran 30, 42 and 60-qubit circuits.

Each of the circuits prepares a state that has 1 bitstring with O(1) probability, while every other bitstring has a significantly (exponentially) smaller probability. 
Part of the challenge was to run all the circuits on the BlueQubit GPU clusters, which only allows circuit compilation upto 36 qubits. In order to do so I had to develop techniques that would allow me to compile 42 and 60-qubit circuits onto the BlueQUbit 36 qubit support GP.

I approach this problem as an optimization challenge for the provided Qasam file. The primary constraint is that the total number of qubits must be less than or equal to 36 to run the circuit on BlueQubit GPUs. However, the given circuits have 42 and 60 qubits respectively, which precludes direct execution on the BQ GPUs.

To address this limitation, I map the original Qasam circuit to a graph representation of all the operations involved. Since the given code utilizes rotation gates for single-qubit operations and CZ gates for two-qubit operations, I can represent the circuit as a graph where CZ gates correspond to edges.

## Optimization Process

1. **Graph Mapping**: Convert the circuit into a graph representation.
2. **Edge and Node Reduction**: Identify and combine repeated edges and nodes.
3. **Graph Optimization**: Utilize the "networkx" library to further optimize and reduce the number of nodes in the circuit.
4. **Circuit Reconstruction**: Convert the optimized graph back into a quantum circuit.

## Execution and Analysis

Once the circuit is optimized, I run it on the BlueQubit platform. The state with the highest number of counts reveals the hidden bitstring of the original circuit. This approach ensures that no information is lost during the "networkx" optimization process.

By employing this method, we can effectively reduce the qubit count to meet the BlueQubit GPU requirements while preserving the essential characteristics and outcomes of the original circuit.
This code is implementing a quantum circuit optimization process using cluster state graphs. Let's break down the main components and their functions:

## Conversion and QASM Generation

The `convert_cluster_graph_to_qasm` function converts an optimized cluster graph back into a QuantumCircuit and then to QASM (Quantum Assembly Language) format:

1. It creates a QuantumCircuit with the same number of qubits as nodes in the cluster graph.
2. Applies Hadamard gates (H) to all qubits.
3. Applies Controlled-Z (CZ) gates based on the edges in the cluster graph.
4. Returns the resulting QuantumCircuit.

The `save_qasm` function saves the QASM representation of a QuantumCircuit to a file.

## Cluster Graph Optimization

The `optimize_cluster_graph` function performs several optimization steps on the cluster graph:

1. **Remove isolated nodes**: Eliminates nodes with no connections.
2. **Merge nodes with degree 2**: Simplifies the graph by connecting neighbors of degree-2 nodes directly.
3. **Merge leaf nodes with their parents**: Reduces the number of leaf nodes (nodes with only one connection).
4. **Merge nodes in linear chains**: Simplifies linear sequences of nodes by connecting the endpoints directly.
5. **Re-label nodes**: Ensures consecutive numbering of nodes after optimization.

## Visualization and Analysis

The code also includes steps to visualize and analyze the cluster graphs:

1. Converts the original quantum circuit to a cluster graph.
2. Prints and visualizes the original cluster graph.
3. Optimizes the cluster graph using the `optimize_cluster_graph` function.
4. Prints and visualizes the optimized cluster graph.

## Key Points

- The optimization process aims to reduce the number of nodes and simplify the graph structure while maintaining the essential quantum operations.
- The code uses the NetworkX library for graph operations and manipulations.
- The optimization can potentially lead to more efficient quantum circuits by reducing the number of required qubits and gates.

This optimization process is particularly useful in quantum computing for simplifying complex quantum circuits, potentially leading to more efficient implementations on quantum hardware with limited resources.

