# Define the process and burst time data
processes = [
    "P1", "P2", "P3", "P4", "P5", "P6", "P7", "P8"
]

burst_times = {
    "P1": [5, 27, 3, 31, 5, 43, 4, 18, 6, 22, 4, 26, 3, 24, 4],
    "P2": [4, 48, 5, 44, 7, 42, 12, 37, 9, 76, 4, 41, 9, 31, 7, 43, 8],
    "P3": [8, 33, 12, 41, 18, 65, 14, 21, 4, 61, 15, 18, 14, 26, 5, 31, 6],
    "P4": [3, 35, 4, 41, 5, 45, 3, 51, 4, 61, 5, 54, 6, 82, 5, 77, 3],
    "P5": [16, 24, 17, 21, 5, 36, 16, 26, 7, 31, 13, 28, 11, 21, 6, 13, 3, 11, 4],
    "P6": [11, 22, 4, 8, 5, 10, 6, 12, 7, 14, 9, 18, 12, 24, 15, 30, 8], 
    "P7": [14, 46, 17, 41, 11, 42, 15, 21, 4, 32, 7, 19, 16, 33, 10],
    "P8": [4, 14, 5, 33, 6, 51, 14, 73, 16, 87, 6]
}

# Define arrival and completion times for each process
arrival_times = {
    "P1": 0,  "P2": 0, "P3": 0, "P4": 0, "P5": 0, "P6": 0, "P7": 0, "P8": 0
}

completion_times = {
    "P1": 0,  "P2": 0, "P3": 0, "P4": 0, "P5": 0, "P6": 0, "P7": 0, "P8": 0
}

# Define a function to calculate average turnaround time, average waiting time, CPU utilization, and average response time
def calculate_metrics(burst_times, processes, arrival_times, completion_times):
    n = len(processes)
    turnaround_times = {}
    waiting_times = {}
    response_times = {}
    current_time = 0
   
    for process in processes:
        # Calculate response time as the time when the process enters the CPU for the first time
        if process not in response_times:
            response_times[process] = current_time

        # Capture CPU burst time and I/O time
        burst_info = burst_times[process]
        cpu_burst = burst_info.pop(0)  # Get and remove the first element as CPU burst time
        io_time = burst_info.pop(0)    # Get and remove the first element as I/O time

        current_time += cpu_burst

        # Calculate turnaround time as the sum of burst times and I/O times
        waiting_times[process] =  cpu_burst * io_time
        # Calculate waiting time as turnaround time - CPU burst time - I/O time
        turnaround_times[process] = waiting_times[process] + cpu_burst - io_time
        # Update completion time for the process
        completion_times[process] = current_time

    # Calculate CPU utilization
    total_execution_time = max(completion_times.values())
    total_waiting_time = sum(waiting_times.values())
    cpu_utilization = total_execution_time + total_waiting_time / 100
    # Calculate average turnaround time, average waiting time, and average response time
    average_turnaround_time = sum(turnaround_times.values()) / n
    average_waiting_time = sum(waiting_times.values()) / n
    average_response_time = sum(response_times.values()) / n

    return turnaround_times, waiting_times, response_times, average_turnaround_time, average_waiting_time, cpu_utilization, average_response_time

# Calculate metrics for FCFS
fcfs_order = processes.copy()
turnaround_times, waiting_times, response_times, avg_turnaround, avg_waiting, cpu_utilization, avg_response = calculate_metrics(burst_times, fcfs_order, arrival_times, completion_times)

print("First-Come-First-Served (FCFS) Times:")
print("Process\tWaiting Time\tTurnaround Time\tResponse Time")
for process in fcfs_order:
    print(f"{process}\t{waiting_times[process]}\t\t{turnaround_times[process]}\t\t{response_times[process]}")
print(f"Average Turnaround Time: {avg_turnaround:.2f}")
print(f"Average Waiting Time: {avg_waiting:.2f}")
print(f"CPU Utilization: {cpu_utilization:.2f}%")
print(f"Average Response Time: {avg_response:.2f}\n")

# Calculate metrics for SJF
sjf_order = sorted(processes, key=lambda process: sum(burst_times[process]))
sjf_turnaround_times, sjf_waiting_times, sjf_response_times, sjf_avg_turnaround, sjf_avg_waiting, sjf_cpu_utilization, sjf_avg_response = calculate_metrics(burst_times, fcfs_order, arrival_times, completion_times)
print("Shortest Job First (SJF) Times:")
print("Process\tTurnaround Time\tWaiting Time\tResponse Time")
for process in sjf_order:
    print(f"{process}\t{sjf_turnaround_times[process]}\t\t{sjf_waiting_times[process]}\t\t{sjf_response_times[process]}")
print(f"Average Turnaround Time: {sjf_avg_turnaround:.2f}")
print(f"Average Waiting Time: {sjf_avg_waiting:.2f}")
print(f"CPU Utilization: {sjf_cpu_utilization:.2f}%")
print(f"Average Response Time: {sjf_avg_response:.2f}\n")

# Define the MLFQ parameters
mlfq_queues = [
    {"queue": ["P1", "P2"], "time_quantum": 5},
    {"queue": ["P3", "P4"], "time_quantum": 10},
    {"queue": processes, "time_quantum": None}
]

# Function to run MLFQ simulation
def run_mlfq_simulation(burst_times, mlfq_queues):
    current_time = 0
    for queue_info in mlfq_queues:
        queue = queue_info["queue"]
        time_quantum = queue_info["time_quantum"]
        for process in queue:
            if time_quantum is not None:
                for _ in range(time_quantum):
                    if burst_times[process]:  # Check if burst time list is not empty
                        burst_times[process][0] -= 1
                        if not burst_times[process][0]:
                            burst_times[process].pop(0)
                            turnaround_times[process] = current_time + 1
                            if process not in waiting_times:
                                waiting_times[process] = current_time
                            break
                    current_time += 1
                if burst_times[process]:  # Check if burst time list is not empty
                    queue_info_next = mlfq_queues[mlfq_queues.index(queue_info) + 1]
                    queue_info_next["queue"].append(process)
            else:
                for burst_time in burst_times[process]:
                    current_time += burst_time
                    turnaround_times[process] = current_time + 1
                    if process not in waiting_times:
                        waiting_times[process] = current_time

# Run MLFQ simulation
mlfq_order = []
run_mlfq_simulation(burst_times, mlfq_queues)
for queue_info in mlfq_queues:
    mlfq_order.extend(queue_info["queue"])

mlfq_turnaround_times, mlfq_waiting_times, mlfq_response_times, mlfq_avg_turnaround, mlfq_avg_waiting, mlfq_cpu_utilization, mlfq_avg_response = calculate_metrics(burst_times, fcfs_order, arrival_times, completion_times)

print("Multilevel Feedback Queue (MLFQ) Times:")
print("Process\tTurnaround Time\tWaiting Time\tResponse Time")
for process in mlfq_order:
    if process not in completion_times:
        continue  # Skip processes that have already been processed
    print(f"{process}\t{mlfq_turnaround_times[process]}\t\t{mlfq_waiting_times[process]}\t\t{mlfq_response_times[process]}")
    completion_times.pop(process)  # Remove the process from the completion_times

print(f"Average Turnaround Time: {mlfq_avg_turnaround:.2f}")
print(f"Average Waiting Time: {mlfq_avg_waiting:.2f}")
print(f"CPU Utilization: {mlfq_cpu_utilization:.2f}%")
print(f"Average Response Time: {mlfq_avg_response:.2f}")









# Define the process and burst time data
processes = [
    "P1", "P2", "P3", "P4", "P5", "P6", "P7", "P8"
]

burst_times = {
    "P1": [5, 27, 3, 31, 5, 43, 4, 18, 6, 22, 4, 26, 3, 24, 4],
    "P2": [4, 48, 5, 44, 7, 42, 12, 37, 9, 76, 4, 41, 9, 31, 7, 43, 8],
    "P3": [8, 33, 12, 41, 18, 65, 14, 21, 4, 61, 15, 18, 14, 26, 5, 31, 6],
    "P4": [3, 35, 4, 41, 5, 45, 3, 51, 4, 61, 5, 54, 6, 82, 5, 77, 3],
    "P5": [16, 24, 17, 21, 5, 36, 16, 26, 7, 31, 13, 28, 11, 21, 6, 13, 3, 11, 4],
    "P6": [11, 22, 4, 8, 5, 10, 6, 12, 7, 14, 9, 18, 12, 24, 15, 30, 8], 
    "P7": [14, 46, 17, 41, 11, 42, 15, 21, 4, 32, 7, 19, 16, 33, 10],
    "P8": [4, 14, 5, 33, 6, 51, 14, 73, 16, 87, 6]
}

# Define arrival and completion times for each process
arrival_times = {
    "P1": 0,  "P2": 0, "P3": 0, "P4": 0, "P5": 0, "P6": 0, "P7": 0, "P8": 0
}

completion_times = {
    "P1": 0,  "P2": 0, "P3": 0, "P4": 0, "P5": 0, "P6": 0, "P7": 0, "P8": 0
}

# Define a function to calculate average turnaround time, average waiting time, CPU utilization, and average response time
def calculate_metrics(burst_times, processes, arrival_times, completion_times):
    n = len(processes)
    turnaround_times = {}
    waiting_times = {}
    response_times = {}
    current_time = 0
   
    for process in processes:
        # Calculate response time as the time when the process enters the CPU for the first time
        if process not in response_times:
            response_times[process] = current_time

        # Capture CPU burst time and I/O time
        burst_info = burst_times[process]
        cpu_burst = burst_info.pop(0)  # Get and remove the first element as CPU burst time
        io_time = burst_info.pop(0)    # Get and remove the first element as I/O time

        current_time += cpu_burst

        # Calculate turnaround time as the sum of burst times and I/O times
        waiting_times[process] =  cpu_burst * io_time
        # Calculate waiting time as turnaround time - CPU burst time - I/O time
        turnaround_times[process] = waiting_times[process] + cpu_burst - io_time
        # Update completion time for the process
        completion_times[process] = current_time

    # Calculate CPU utilization
    total_execution_time = max(completion_times.values())
    total_waiting_time = sum(waiting_times.values())
    cpu_utilization = total_execution_time + total_waiting_time / 100
    # Calculate average turnaround time, average waiting time, and average response time
    average_turnaround_time = sum(turnaround_times.values()) / n
    average_waiting_time = sum(waiting_times.values()) / n
    average_response_time = sum(response_times.values()) / n

    return turnaround_times, waiting_times, response_times, average_turnaround_time, average_waiting_time, cpu_utilization, average_response_time

# Calculate metrics for FCFS
fcfs_order = processes.copy()
turnaround_times, waiting_times, response_times, avg_turnaround, avg_waiting, cpu_utilization, avg_response = calculate_metrics(burst_times, fcfs_order, arrival_times, completion_times)

print("First-Come-First-Served (FCFS) Times:")
print("Process\tWaiting Time\tTurnaround Time\tResponse Time")
for process in fcfs_order:
    print(f"{process}\t{waiting_times[process]}\t\t{turnaround_times[process]}\t\t{response_times[process]}")
print(f"Average Turnaround Time: {avg_turnaround:.2f}")
print(f"Average Waiting Time: {avg_waiting:.2f}")
print(f"CPU Utilization: {cpu_utilization:.2f}%")
print(f"Average Response Time: {avg_response:.2f}\n")

# Calculate metrics for SJF
sjf_order = sorted(processes, key=lambda process: sum(burst_times[process]))
sjf_turnaround_times, sjf_waiting_times, sjf_response_times, sjf_avg_turnaround, sjf_avg_waiting, sjf_cpu_utilization, sjf_avg_response = calculate_metrics(burst_times, fcfs_order, arrival_times, completion_times)
print("Shortest Job First (SJF) Times:")
print("Process\tTurnaround Time\tWaiting Time\tResponse Time")
for process in sjf_order:
    print(f"{process}\t{sjf_turnaround_times[process]}\t\t{sjf_waiting_times[process]}\t\t{sjf_response_times[process]}")
print(f"Average Turnaround Time: {sjf_avg_turnaround:.2f}")
print(f"Average Waiting Time: {sjf_avg_waiting:.2f}")
print(f"CPU Utilization: {sjf_cpu_utilization:.2f}%")
print(f"Average Response Time: {sjf_avg_response:.2f}\n")

# Define the MLFQ order and time quantum for each queue
mlfq_order = [
    ["P1", "P2", "P3", "P4"],  # Queue 1 (RR with Tq = 5)
    ["P5", "P6", "P7"],        # Queue 2 (RR with Tq = 10)
    ["P8"]                      # Queue 3 (FCFS)
]

time_quantum = [5, 10]

# Define a function to calculate metrics for MLFQ
def calculate_mlfq_metrics(burst_times, mlfq_order, arrival_times, time_quantum):
    # Initialize variables for MLFQ metrics
    mlfq_turnaround_times = {}
    mlfq_waiting_times = {}
    mlfq_response_times = {}
    mlfq_completion_times = {}  # Store completion times for each process
    mlfq_avg_turnaround = 0
    mlfq_avg_waiting = 0
    mlfq_cpu_utilization = 0
    mlfq_avg_response = 0
    current_time = 0

    for i, queue in enumerate(mlfq_order):
        for process in queue:
            if process not in mlfq_response_times:
                mlfq_response_times[process] = current_time

            burst_info = burst_times[process]
            remaining_burst_time = burst_info[0]

            if i < len(time_quantum):  # Check if there's a corresponding time quantum
                current_time += min(remaining_burst_time, time_quantum[i])
                burst_info[0] -= min(remaining_burst_time, time_quantum[i])

                if remaining_burst_time > time_quantum[i]:
                    # Move the process to the end of the current queue
                    queue.append(process)
                else:
                    # Move the process to the next queue
                    if i < len(mlfq_order) - 1:
                        mlfq_order[i + 1].append(process)
            else:
                # FCFS scheduling for the lowest priority queue
                current_time += remaining_burst_time
                burst_info.pop(0)  # Remove the CPU burst time

            if process not in mlfq_completion_times:
                mlfq_completion_times[process] = current_time

            mlfq_waiting_times[process] = mlfq_completion_times[process] + current_time
            mlfq_turnaround_times[process] = mlfq_waiting_times[process] + sum(burst_info)
            mlfq_avg_turnaround += mlfq_turnaround_times[process]
            mlfq_avg_waiting += mlfq_waiting_times[process]
            mlfq_avg_response += mlfq_response_times[process]

    total_execution_time = max(mlfq_completion_times.values())
    total_waiting_time = sum(mlfq_waiting_times.values())
    mlfq_cpu_utilization = (total_execution_time / current_time) * 100

    return mlfq_turnaround_times, mlfq_waiting_times, mlfq_response_times, mlfq_avg_turnaround / len(processes), mlfq_avg_waiting / len(processes), mlfq_cpu_utilization, mlfq_avg_response / len(processes)

# Calculate metrics for MLFQ
mlfq_turnaround_times, mlfq_waiting_times, mlfq_response_times, mlfq_avg_turnaround, mlfq_avg_waiting, mlfq_cpu_utilization, mlfq_avg_response = calculate_mlfq_metrics(burst_times, mlfq_order, arrival_times, time_quantum)

print("Multilevel Feedback Queue (MLFQ) Times:")
print("Process\tTurnaround Time\tWaiting Time\tResponse Time")
for process in processes:
    print(f"{process}\t{mlfq_turnaround_times[process]}\t\t{mlfq_waiting_times[process]}\t\t{mlfq_response_times[process]}")
print(f"Average Turnaround Time: {mlfq_avg_turnaround:.2f}")
print(f"Average Waiting Time: {mlfq_avg_waiting:.2f}")
print(f"CPU Utilization: {mlfq_cpu_utilization:.2f}%")
print(f"Average Response Time: {mlfq_avg_response:.2f}")




