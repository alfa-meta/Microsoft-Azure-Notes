
Asynchronous programming -

Multithreading -



## Summary
- The tools for using asynchronous operations are also good for using multithreaded operations.
- The high-performance code that benefits from multithreading is also likely to benefit from using asynchronous operations.
- In UI apps, when using await in the UI thread, the code after `await` will also run in the UI thread.
- In all other cases, the code after await will run in the thread pool.
- If the code calling await is running in the thread pool, the code after the `await` might run in a different thread in the thread pool.
- You can't use the `await` inside the code block of a `lock` statement. The best solution is to rearrange your code and move the `await` outside of the `lock` block.