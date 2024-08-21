# Graph-Based-File-Management

## Overview

This project simulates a distributed graph database system designed to handle client requests through various components, including a load balancer, primary server, secondary servers, a cleanup process, and multiple clients. The system supports both read and write operations on graph data, with a focus on multithreading and inter-process communication (IPC) using message queues and shared memory.

## Components

### 1. Load Balancer (`load_balancer.c`)

- **Function**: Receives client requests and distributes them to the appropriate servers.
- **Details**:
  - Uses a single message queue for communication.
  - Forwards write requests to the primary server.
  - Distributes read requests between two secondary servers based on request number (odd/even).

### 2. Primary Server (`primary_server.c`)

- **Function**: Handles write operations for adding or modifying graph files.
- **Details**:
  - Processes write requests using threads.
  - Reads graph data from shared memory and updates files accordingly.
  - Responds to clients with status messages.

### 3. Secondary Servers (`secondary_server.c`)

- **Function**: Handles read operations, including DFS and BFS traversals.
- **Details**:
  - Each server spawns threads for processing client requests.
  - Performs concurrent traversals of acyclic graphs as specified by the client.
  - Sends traversal results back to the client via the message queue.

### 4. Cleanup Process (`cleanup.c`)

- **Function**: Manages the termination of the system.
- **Details**:
  - Monitors for a termination request from the user.
  - Signals the load balancer to shut down, which then informs all servers.
  - Ensures proper cleanup and termination of all processes.

### 5. Client

- **Function**: Interacts with the system to send requests and receive responses.
- **Details**:
  - Options include adding/modifying graphs and performing DFS/BFS traversals.
  - Uses shared memory for data transfer and the message queue for receiving responses.
  - Handles input prompts and displays results.

## Graph File Format

Graph files follow the naming convention `Gx.txt`, where `x` is a number. Each file contains:
- An integer `n` specifying the number of nodes in the graph.
- An `n x n` adjacency matrix where each row represents a node and its edges.

### Sample Graph File

4
0 1 0 1
1 0 1 0
0 1 0 1
1 0 1 0

r
Copy code

## Instructions

### Compilation

Compile all C files using a POSIX-compliant compiler:

```bash
gcc -o load_balancer load_balancer.c -lpthread
gcc -o primary_server primary_server.c -lpthread
gcc -o secondary_server secondary_server.c -lpthread
gcc -o cleanup cleanup.c -lpthread
Execution
Start the Load Balancer:

bash
Copy code
./load_balancer
Start the Secondary Servers:

bash
Copy code
./secondary_server 1
./secondary_server 2
Start the Primary Server:

bash
Copy code
./primary_server
Start the Cleanup Process:

bash
Copy code
./cleanup
Client Interaction
Clients interact with the system by choosing from the following options:

Add a New Graph: Sends graph data to the primary server.
Modify an Existing Graph: Updates an existing graph file.
Perform DFS: Executes a Depth-First Search traversal on a graph.
Perform BFS: Executes a Breadth-First Search traversal on a graph.
Termination
To terminate the system:

Use the cleanup process to signal the load balancer.
The load balancer will notify all servers to shut down and clean up resources.
