# AdvProg-Module10
Name : Siti Shofi Nadhifa <br>
NPM : 2306152172 <br>
Class : AdPro B

### 1.2 Understanding how it works
![result](/images/image-1.png)
"Shofi's Computer: hey hey" is printed first because it runs synchronously right after the task is spawned but before the executor starts processing tasks. The `spawner.spawn(...)` call only queues the asynchronous task without executing it immediately. Since `println!("Shofi's Computer: hey hey")` is a regular synchronous statement, it executes and prints right away. When `executor.run()` is called, the task is pulled from the queue and begins execution, printing "Shofi's Computer: howdy!" and eventually "Shofi's Computer: done!" after awaiting the timer.