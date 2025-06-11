
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

Thread has significant overhead.

Blocking Thread - a thread that stops execution (waits) until a specific operation completes.
	During this time, it does not perform other tasks.

Context Switch - is the process by which a CPU switches from executing one process (or thread) to another.
	During this switch, the state (context) of the current process is saved so it can be resumed later, and the state of the next scheduled process is loaded.

Context Switching has significant overhead.

Thread Affinity - setting a thread to run on a specific core.

Asynchronous Programming - frees up the thread while operations are taking place by asking the system to send a notification when the operation ends instead of waiting.
	Called non-blocking operation.