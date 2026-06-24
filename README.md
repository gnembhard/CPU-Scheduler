# CPU Scheduling Simulator

A Python-based simulation of several classical CPU scheduling algorithms used in operating systems. The project compares scheduling strategies by calculating performance metrics such as turnaround time, waiting time, response time, and CPU utilization.

## Implemented Scheduling Algorithms

### First Come First Serve (FCFS)
- Processes are executed in the order they arrive.
- Non-preemptive scheduling algorithm.
- Uses the predefined process list order (`P1 → P8`).

### Shortest Job First (SJF)
- Processes are sorted based on their total burst time.
- The process with the shortest total execution requirement is scheduled first.
- Non-preemptive implementation.

### Multi-Level Feedback Queue (MLFQ)
The scheduler uses three priority queues:

| Queue | Scheduling Policy | Time Quantum |
|--------|-------------------|-------------|
| Queue 1 | Round Robin | 5 |
| Queue 2 | Round Robin | 10 |
| Queue 3 | FCFS | Unlimited |

Processes that exceed their allotted time quantum are moved to lower-priority queues.

## Performance Metrics

The simulator calculates:

- Turnaround Time
- Waiting Time
- Response Time
- CPU Utilization
- Average Turnaround Time
- Average Waiting Time
- Average Response Time

## Process Dataset

The simulation includes eight processes:

- P1
- P2
- P3
- P4
- P5
- P6
- P7
- P8

Each process contains multiple CPU burst and I/O burst cycles to simulate realistic operating system workloads.

## Concepts Demonstrated

- Process scheduling
- CPU burst and I/O burst management
- Round Robin scheduling
- Queue prioritization
- Performance evaluation of scheduling policies
- Preemptive and non-preemptive scheduling

