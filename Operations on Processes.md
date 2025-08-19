
# Process Creation
- during the course of execution, a process can create new processes. 
- the creating process is called a parent process, and the new ones are called the children of that process. each of these may in turn create other processes, forming a tree of processes. 
- OSes identify processes by a process identifier or pid, which is typically an integer number. 
	- the pid provides a unique value for each process in the system and can be used as an index.
- the systemd process (which always has a pid of 1) serves as the root parent process for all user processes, and is the first user process created when the system boots.
	- once the system has booted, it creates processes important processes related to providing additional services.
- in UNIX and Linux systems, a list of processes can be obtained using the `ps` command.:
	- ``` ps -el
- in general, when a process creates a child process, that child process will need certain resources to accomplish its task. 
	- a child process may be able to obtain its resources directly from the OS, or
	- it may be constrained to a subset of the resources of the parent process.
		- restricting a child process to a subset of the parent's resources prevents any process from overloading the system by creating too many child processes.
	- the parent can also pass initialization data to the child process.
- when a process creates a new process, two possibilities for execution can exist:
	 1. the parent continues to execute concurrently with its children
	2. the parent waits until some or all of its children have terminated.
- there are also two address-space possibilities:
	1. the child process is a duplicate of the parent process (it has the same program and data as the parent)
	2. the child process has a new program loaded into it.


# Process Termination
a process terminates when it finishes executing its final statement and asks the OS to delete it by using the `exit()` system call. At that point, the process may return a status value (typically an integer) to its waiting parent process (via the `wait()` system call). All the resources of the process--including physical and virtual memory, open files, and I/O buffers--are deallocated and reclaimed by the os.
- termination can also occur:
	- a process can cause the termination of another process via an appropriate system call. usually, such as system call can only be invoked only by the parent of the process that is to be terminated. 
	- a parent may terminate its children for a variety of reasons:
		- the child has exceeded the its resource usage
		- the task assigned to the child is no longer required
		- the parent is exiting, and the OS does not allow a child to exist if the parent terminates.
			- this is known as **cascading termination.**
- a parent process may wait for the termination of a child by using the `wait()` system call. it is passed a parameter that allows the parent to obtain the exit status of the child. this system call also returns the pid of the child. 
- when a process terminates, its resources are deallocated by the OS. however, its entry in the process table must remain there until the parent calls `wait()`, because the process table contains the process's exit status.
- a process that has terminated but whose parent has not yet called wait. is known as a zombie process. all processes transition to this state when they terminate, but generally they exist as zombies only briefly. one the parent calls wait, the pid of the zombie and its entry in the process table are released.
- if a parent did not invoke wait and instead terminated, its children are now orphan processes. traditional UNIX systems reassigns the orphans to `init` as the parent process. the init process periodically invokes wait, thereby allowing any orphans' pid to be released.

## Android Process Hierarchy
- Mobile OSes may have to terminate existing processes to reclaim limited system resources.
- rather than terminating an arbitrary process, Android has identified an important hierarchy of processes. From most to least important, the hierarchy of process classifications is as follows:
	1. Foreground process - the current process visible on the screen, representing the application the user is currently interacting with
	2. Visible process - a process that is not directly visible on the foreground but that is performing an activity that the foreground process is referring to (that is, a process performing an activity whose status is displayed on the foreground process)
	3. Service process - a process that is similar to a background process but is performing an activity that is apparent to the user (such as streaming music)
	4. Background process - a process that may be performing an activity but is not apparent to the user
	5. Empty process - a process that holds no active components associated with any application
- if system resources must be claimed, Android will first terminate empty processes going up the hierarchy.


## Chrome Browser
- Many web browsers ran as a single process (some still do)
- Google Chrome is a multiprocess with 3 different types of processes:
	- Browser process manages the UI, disk and network I/O
	- Renderer process renders web pages, deals with HTML, JS, etc... As a general rule, a new renderer process is created for each website open in a new tab, and so several renderer processes may be active at the same time.
		- Runs in sandbox mode restricting disk and networking I/O, minimizing effect of security exploits
	- Plug-in process for each type of plug-in (such as Flash or QuickTime) in use. Plug-in processes contain the code for the plug-in as well as additional code that enables the plug-in to communicate with associated renderer processes and the browser process
- The advantage of this multiprocess approach is that websites run in isolation. if a website crashes, only its renderer process is affected; all other processes remain unharmed.





