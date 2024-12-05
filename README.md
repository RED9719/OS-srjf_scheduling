# OS-srjf_scheduling
import pandas as pd

class Process:
    def __init__(self, pid, arrival_time, burst_time):
        self.pid = pid
        self.arrival_time = arrival_time
        self.burst_time = burst_time
        self.remaining_time = burst_time
        self.start_time = -1
        self.completion_time = 0
        self.turnaround_time = 0
        self.waiting_time = 0

def srjf_scheduling(processes):
    n = len(processes)
    processes.sort(key=lambda x: x.arrival_time)
    
    current_time = 0
    completed = 0
    ready_queue = []
    while completed != n:
        for process in processes:
            if process.arrival_time == current_time:
                ready_queue.append(process)

        ready_queue = [p for p in ready_queue if p.remaining_time > 0]
        if ready_queue:
            ready_queue.sort(key=lambda x: (x.remaining_time, x.arrival_time))
            current_process = ready_queue[0]
            
            if current_process.start_time == -1:
                current_process.start_time = current_time

            current_process.remaining_time -= 1
            current_time += 1
            
            if current_process.remaining_time == 0:
                current_process.completion_time = current_time
                current_process.turnaround_time = current_process.completion_time - current_process.arrival_time
                current_process.waiting_time = current_process.turnaround_time - current_process.burst_time
                completed += 1
        else:
            current_time += 1

def print_process_info(processes):
    process_info = []
    for process in processes:
        process_info.append([process.pid, process.arrival_time, process.burst_time,
                             process.start_time, process.completion_time,
                             process.turnaround_time, process.waiting_time])

    df = pd.DataFrame(process_info, columns=["Process ID", "Arrival Time", "Burst Time",
                                             "Start Time", "Completion Time",
                                             "Turnaround Time", "Waiting Time"])
    print(df)

def main():
    processes = []
    num_processes = int(input("Enter the number of processes: "))

    for i in range(num_processes):
        pid = int(input(f"\nEnter Process ID for process {i + 1}: "))
        arrival_time = int(input(f"Enter Arrival Time for process {pid}: "))
        burst_time = int(input(f"Enter Burst Time for process {pid}: "))
        processes.append(Process(pid, arrival_time, burst_time))

    srjf_scheduling(processes)
    print("\nProcess Information after SRJF Scheduling:")
    print_process_info(processes)

if __name__ == "__main__":
    main()
