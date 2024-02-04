def calculate_hrr(process, current_time):
    wait_time = current_time - process['arrival_time']
    response_ratio = (wait_time + process['burst_time']) / process['burst_time']
    return response_ratio

def hrrf_scheduling(processes):
    schedule = []
    current_time = 0
    total_waiting_time = 0

    while processes:
        eligible_processes = [p for p in processes if p['arrival_time'] <= current_time]
        
        if not eligible_processes:
            current_time += 1
            continue

        hrr_values = [calculate_hrr(process, current_time) for process in eligible_processes]
        selected_process = eligible_processes[hrr_values.index(max(hrr_values))]

        schedule.append(selected_process['name'])
        total_waiting_time += current_time - selected_process['arrival_time']

        current_time += selected_process['burst_time']
        processes.remove(selected_process)

    average_waiting_time = total_waiting_time / len(schedule)

    return schedule, average_waiting_time

def main():
    num_processes = int(input("Enter the number of processes: "))
    processes = []

    for i in range(num_processes):
        name = input(f"Enter the name of process {chr(ord('A')+i)}: ")
        arrival_time = int(input(f"Enter arrival time for process {name}: "))
        burst_time = int(input(f"Enter burst time for process {name}: "))
        processes.append({'name': name, 'arrival_time': arrival_time, 'burst_time': burst_time})

    schedule, avg_waiting_time = hrrf_scheduling(processes)

    print("\nSchedule after applying HRRF algorithm:")
    print(" -> ".join(schedule))
    print(f"\nAverage Waiting Time: {avg_waiting_time:.2f}")

if __name__ == "__main__":
    main()
