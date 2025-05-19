# AdvProg-Module10
Name : Siti Shofi Nadhifa <br>
NPM : 2306152172 <br>
Class : AdPro B

### 1.2 Understanding how it works
![result](/images/image-1.png)
"Shofi's Computer: hey hey" is printed first because it runs synchronously right after the task is spawned but before the executor starts processing tasks. The `spawner.spawn(...)` call only queues the asynchronous task without executing it immediately. Since `println!("Shofi's Computer: hey hey")` is a regular synchronous statement, it executes and prints right away. When `executor.run()` is called, the task is pulled from the queue and begins execution, printing "Shofi's Computer: howdy!" and eventually "Shofi's Computer: done!" after awaiting the timer.

### 1.3 Multiple Spawn and removing drop
- `drop(spawner)` is executed
![drop-on](/images/image-2.png)
- `drop(spawner)` is not executed (commented)
![drop-off](/images/image-3.png)
From the output produced, we can observe that having more spawner calls results in more tasks being enqueued into the `task_sender`, which functions as a message queue. Each call to `spawner.spawn(...)` creates a new task and sends it to the executor via the channel. The `drop(spawner)` statement acts as a signal that no more tasks will be sent, indicating to the executor that the task production phase has ended. If `drop(spawner)` is commented out, the program does not terminate because the executor remains in a waiting state, expecting additional tasks that will never come. However, in the provided code, all tasks are spawned before `executor.run()` is called, so whether the spawner is dropped or not does not affect which tasks are executed—it only affects whether the program exits cleanly. The executor pulls one task at a time from the queue and attempts to run it. If a task is not yet complete (for example, it awaits a `TimerFuture`), it is re-enqueued to be polled again later. This process repeats until the queue is empty and the spawner is dropped. In terms of execution order, the synchronous `println!("hey hey")` is always printed first, followed by the initial outputs from each task (`howdy`, `howdy2`, `howdy3`) in the order they were spawned. However, the “done” messages (`done`, `done2`, `done3`) appear in a non-deterministic order across different runs. This is because each `TimerFuture` spawns a background thread with a sleep duration, and the actual completion time depends on how the operating system schedules these threads. As a result, the “done” prints occur in a random order. In summary, the executor continuously dequeues tasks, polls them, and either completes or reschedules them, continuing this cycle until all tasks are finished and the spawner is dropped to signal termination.
