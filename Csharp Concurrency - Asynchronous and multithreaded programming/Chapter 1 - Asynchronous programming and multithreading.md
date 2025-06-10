
Multithreading - allows a computer to appear as if it is executing several tasks at once.
	Even when the number of tasks exceeds the number of CPU cores.


Single-threaded, single-request flow:
	Web request Received
	Processing
	Read file (waiting)
	Processing
	Send response

Thread - smallest unit of execution within a process.
	Runs a sequence of instructions independently but shares the same memory space and resources with other threads in the same process.

Blocking Thread - a thread that stops execution (waits) until a specific operation completes.
	During this time, it does not perform other tasks.

Context Switch - is the process by which a CPU switches from executing one process (or thread) to another.
	During this switch, the state (context) of the current process is saved so it can be resumed later, and the state of the next scheduled process is loaded.

