# Distributed Graph Database System

## Overview

This assignment involves simulating a distributed graph database system with the following components:

- **Load Balancer**: Distributes client requests to the appropriate servers.
- **Primary Server**: Handles write operations (adding or modifying graphs).
- **Secondary Servers**: Handle read operations (DFS/BFS traversals).
- **Cleanup Process**: Manages the shutdown of the system.

## System Architecture

1. **Clients**: Send requests to the Load Balancer.
2. **Load Balancer**: Forwards requests to the Primary Server (write operations) or Secondary Servers (read operations).
3. **Primary Server**: Updates graph files based on client requests.
4. **Secondary Servers**: Perform DFS/BFS traversals on graph files.
5. **Cleanup Process**: Initiates the shutdown of all components.

## Components

### Graph Database

- Graph files are named `Gx.txt`, where `x` is a number (e.g., `G1.txt`).
- Each file contains:
  - A single integer `n` representing the number of nodes.
  - An `n x n` adjacency matrix.

### Client Process

- Options:
  1. Add a new graph to the database
  2. Modify an existing graph
  3. Perform DFS on an existing graph
  4. Perform BFS on an existing graph
- Requests are sent to the Load Balancer using a single message queue.
- For write operations, clients also write graph data to a shared memory segment.
- For read operations, clients specify a starting vertex for traversal.

### Load Balancer

- Receives requests from clients via a single message queue.
- Forwards:
  - Write requests to the Primary Server.
  - Odd-numbered read requests to Secondary Server 1.
  - Even-numbered read requests to Secondary Server 2.

### Primary Server

- Handles write requests (add or modify graph files).
- Creates a thread to process each request.
- Updates graph files based on client-provided data.

### Secondary Servers

- Handle read requests (DFS/BFS traversals).
- Each read request is processed by a thread.
- DFS uses a new thread for each unvisited node, ensuring depth-wise traversal.
- BFS processes nodes level by level, creating threads for nodes at each level.

### Cleanup Process

- Displays a menu to terminate the application.
- Signals the Load Balancer to start the shutdown sequence.
- Waits for the Load Balancer to terminate all components before exiting.

## Running the Processes

1. **Start the Load Balancer Process**.
2. **Start Two Instances of the Secondary Server**:
   - Secondary Server 1
   - Secondary Server 2
3. **Start the Primary Server Process**.
4. **Start the Cleanup Process**.

## Client Interaction

Clients can interact with the system by choosing from the following options:

1. **Add a New Graph**: Sends graph data to the Primary Server.
2. **Modify an Existing Graph**: Updates an existing graph file.
3. **Perform DFS**: Executes a Depth-First Search traversal on a graph.
4. **Perform BFS**: Executes a Breadth-First Search traversal on a graph.

## Termination

To terminate the system:

1. **Use the Cleanup Process** to signal the Load Balancer.
2. The **Load Balancer** will then initiate the shutdown sequence for all components.
