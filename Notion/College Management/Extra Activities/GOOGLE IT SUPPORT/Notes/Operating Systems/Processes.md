  

Processes are the dynamic execution instances of programs on a computer. When you launch an application, you're essentially starting a process. Each process is assigned a unique identifier called a process ID (PID) to distinguish it from other concurrently running processes. These processes consume hardware resources like CPU and RAM.

Here's a breakdown of the concepts mentioned in the lesson:

1. **Programs vs. Processes:**
    - **Programs:** These are applications or software, like web browsers or word processors.
    - **Processes:** When you execute a program, it becomes a process. You can have multiple processes running for the same program. For example, multiple instances of a web browser or several documents opened in a word processor.
2. **Resource Utilization:**
    - When processes run, they utilize hardware resources such as CPU (Central Processing Unit) and RAM (Random Access Memory).
3. **Computer Performance:**
    - Computers today are generally powerful enough to handle various processes simultaneously, ensuring smooth performance for day-to-day activities like web browsing and watching movies.
4. **Process Management:**
    - Sometimes, processes may misbehave by consuming excessive resources, leading to system slowdown or unresponsiveness. Understanding and managing processes become essential to maintain system stability.
5. **Background Processes (Daemons):**
    - In addition to user-initiated processes, there are background processes or daemons. These processes operate in the background and are essential for the proper functioning of the system. Examples include resource scheduling, logging, and network management.
6. **Kernel Role:**
    - The kernel, a core component of the operating system, manages processes. It allocates resources and ensures the smooth execution of processes.
7. **Process ID (PID):**
    
    - Each process is assigned a unique identifier called a PID. This ID helps the system differentiate between different processes.
    
      
    
      
    

  

In Windows, the process creation and termination mechanisms differ from Linux. Here's a breakdown of the information provided:

1. **Windows Process Creation:**
    - When Windows boots up, the Session Manager Subsystem (smss.exe) is the first non-kernel and user mode process. It sets up essential components for the OS to function.
    - The login process (windlogon.exe) and the client-server runtime subsystem (csrss.exe) are initiated by smss.exe. Csrss.exe handles running the Windows GUI and command-line console.
2. **Parent-Child Relationship in Windows:**
    - In Windows, each new process requires a parent process to inform the operating system about the need for a new process.
    - Child processes inherit certain attributes, such as variables and settings, from their parent. This inherited set of attributes is collectively referred to as an environment.
    - Unlike Linux, Windows processes can operate independently of their parent processes. Even if the parent process is terminated, child processes can continue running.
3. **Example: Creating a Process in Windows (Notepad):**
    - Using PowerShell, a new process for Notepad (notepad.exe) is created. The parent process is PowerShell, and the child process is Notepad.
    - Even if the parent process (PowerShell) is terminated (e.g., by clicking the "X" button), the child process (Notepad) continues to run independently.
4. **Stopping Processes in Windows:**
    - Processes can be stopped using various methods. One way is to use the taskkill utility from the command prompt.
    - The taskkill command can terminate a process by specifying its Process ID (PID). For example, taskkill /PID [PID] sends a termination signal to the process identified by the given PID.
5. **Terminating Notepad Using Taskkill:**
    
    - The process ID of Notepad is obtained, and taskkill /PID [Notepad PID] is used to send a termination signal to the Notepad process.
    
      
    

  

In Linux, processes follow a parent-child relationship, where each process is derived from another process. Here's a summary of the information provided:

1. **Parent-Child Relationship in Linux:**
    - Every process launched in Linux originates from another process, establishing a parent-child relationship.
    - An example is given with the `**less**` command acting as the parent process to the `**grep**` process.
2. **Init Process:**
    - When a computer starts up, the kernel creates a fundamental process called `**init**` with a Process ID (PID) of one.
    - The `**init**` process is responsible for initiating other processes necessary for the system to become operational.

**Termination of Processes:**

- Processes in Linux typically terminate automatically once they complete their tasks.
- Upon termination, a process releases all the resources it was utilizing back to the kernel, making them available for other processes.

  

  

In Linux, understanding and viewing processes can be done using the `**ps**` command. Let's break down the key concepts:

1. **Running** `**ps**` **Command:**
    - `**ps -x**`: Provides a snapshot of current processes running on the system.
    - Output includes columns like PID (Process ID), TTY (Terminal), STAT (Process Status), Time (CPU time), and Command.
2. **Understanding Output:**
    - **PID (Process ID):** Unique ID assigned to each process when launched.
    - **TTY (Terminal):** Associated terminal with the process.
    - **STAT (Process Status):** Indicates if the process is running (R), stopped (T), or in interruptible sleep (S).
    - **Time (CPU time):** Total CPU time consumed by the process.
    - **Command:** Name of the command being executed.
3. **Advanced** `**ps**` **Command:**
    - `**ps -ef**`: Provides more detailed information about all processes, including those run by other users.
    - Additional columns include UID (User ID), PPID (Parent Process ID), C (Number of children processes), S time (Start time).
4. **Searching with** `**grep**`**:**
    - `**ps -ef | grep Chrome**`: Filters processes to show only those containing the name "Chrome."
5. **Viewing Processes as Files:**
    - In Linux, everything is a file, including processes.
    - Explore the `**/proc**` directory, where each subdirectory corresponds to a running process.
    - Each subdirectory contains files with detailed information about the process.
6. **Practicality of** `**/proc**` **Directory:**
    
    - While the `**/proc**` directory provides detailed information, it may not be practical for regular troubleshooting.
    - Stick to using `**ps -ef**` for a quick overview of process information.
    
      
    

  

Signals are a way to indicate to processes that an event happened. Like for example, Ctrl + C exits a process at the system level. Doing this sends a SIGINT signal. The same can be done in Linux with signals like:

  

1. **SIGINT (Signal Interrupt):**
    - **Purpose:** Used to interrupt a process.
    - **Example:** Pressing `**Ctrl + C**` in the terminal sends a SIGINT signal to the currently running process, prompting it to terminate.
2. **SIGKILL (Signal Kill):**
    - **Purpose:** Forces a process to terminate.
    - **Example:** Using the `**kill**` command with the `**9**` option (e.g., `**kill -9 <PID>**`) sends a SIGKILL signal, killing the specified process.
3. **SIGTERM (Signal Terminate):**
    - **Purpose:** Requests a process to terminate gracefully.
    - **Example:** Using the `**kill**` command without the `**9**` option sends a SIGTERM signal, allowing the process to perform cleanup before exiting.
4. **Sending Signals in Practice:**
    - When a process is running in the terminal, `**Ctrl + C**` typically sends a SIGINT signal.
    - The `**kill**` command is used to send signals manually, with options like `**9**` for SIGKILL.
    - **Example:** `**kill -9 <PID>**` sends a SIGKILL signal to the process with the specified PID.
5. **Terminating Processes:**
    
    - Sending signals allows for graceful termination or forceful killing of processes.
    - Choose between SIGTERM and SIGKILL based on whether you want the process to perform cleanup actions before exiting.
    
      
    
      
    
6. **SIGTERM (Signal Terminate):**
    - **Purpose:** Requests a process to terminate gracefully.
    - **Example:** `**kill <PID>**` or `**kill -15 <PID>**` sends a SIGTERM signal, allowing the process to perform cleanup before exiting.
7. **SIGKILL (Signal Kill):**
    - **Purpose:** Forces a process to terminate immediately.
    - **Example:** `**kill -9 <PID>**` sends a SIGKILL signal, killing the specified process without giving it time to clean up.
8. **SIGSTP (Signal Terminal Stop):**
    - **Purpose:** Suspends a process, putting it in a suspended state.
    - **Example:** `**kill -TSTP <PID>**` or `**Ctrl + Z**` sends a SIGSTP signal, pausing the process.
9. **SIGCONT (Signal Continue):**
    - **Purpose:** Resumes the execution of a previously stopped process.
    - **Example:** `**kill -CONT <PID>**` sends a SIGCONT signal, allowing the process to continue its execution.
10. **Sending Signals in Practice:**
    - The `**kill**` command is used to send signals to processes.
    - Commonly used signals include SIGTERM, SIGKILL, SIGSTP, and SIGCONT.
11. **Considerations:**
    - SIGTERM allows processes to perform cleanup before termination.
    - SIGKILL forcefully terminates a process without cleanup.
    - SIGSTP suspends a process, and SIGCONT resumes its execution.
12. **Best Practices:**
    
    - Use SIGTERM for a graceful termination whenever possible.
    - Reserve SIGKILL as a last resort due to its immediate termination without cleanup.
    - SIGSTP can be used to pause a process temporarily.
    
      
    
      
    

For resource monitoring in windows, you can use the task manager, Get-Process, or the resource monitor.

  

For Linux resource monitoring:

1. **Top Command:**
    - **Usage:** Provides real-time information on top processes, CPU usage, memory usage, and more.
    - **Key Fields:** Percentage CPU and Percentage MEM show the resource usage of individual tasks.
    - **Exiting:** Press 'q' to quit the top command.
2. **Identifying Slow Performance:**
    - **Scenario:** If a computer is running slow, check top to identify resource-heavy processes.
    - **Action:** Investigate and potentially terminate processes using excessive resources.
3. **Uptime Command:**
    - **Usage:** Displays current time, system uptime, logged-in users, and load averages.
    - **Load Averages:** Reflect average CPU load over 1, 5, and 15 minute intervals.
4. **LSOF Command:**
    - **Usage:** Lists open files and associated processes.
    - **Scenario:** Useful for troubleshooting issues like "device or resource busy" when ejecting USB drives.
5. **Hardware Utilization Monitoring:**
    - **Monitoring:** Tools like top, uptime, and LSOF help manage processes and monitor hardware utilization.
    - **Future Consideration:** For fleet management, monitoring hardware utilization across multiple machines is valuable.