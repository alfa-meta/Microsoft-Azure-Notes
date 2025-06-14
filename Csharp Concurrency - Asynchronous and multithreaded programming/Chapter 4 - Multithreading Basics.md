





### Summary 
You can run multiple things in parallel. Each one of these things is called a thread.
The program starts with one thread running the `Main` method. This thread is called the main thread.
You can create Threads that are completely under your control with the `Thread` class by creating a `Thread` object, optionally reconfiguring it, and calling the `Thread.Start` method to start running it.
The thread pool is a collection of threads managed by the system and is available for use when you have code to run. It is optimised for short-running tasks and can create new threads as needed.
Traditionally, you run code in the thread pool by using the `ThreadPool.QueueUserWorkItem` method.
A simpler and more `async/await` friendly way to run code in the thread pool is using `Task.Run`.
The main thread and threads you create with the `Thread` class are the only threads that are completely under your control and that you can reconfigure any way you want. However, you should never reconfigure threads created by other components, especially threads managed by the threads pool.
When you access data shared by more than one thread, you have to use a lock; otherwise, different threads may overwrite data written by other threads, leading to incorrect results.
The same also applies to reading shared data. If you don't use locks to synchronise reads as well as writes, you may get stale data and even the results of incomplete writes.
In native UI applications, the thread running the UI is called the UI thread. It is typically the main thread, but it can be a different thread if needed. The UI thread is the only thread that may access windows and other UI controls.
You should avoid blocking the UI thread because this makes the UI freeze.