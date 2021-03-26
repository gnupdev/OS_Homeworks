## Project #3: Implementing synchronization primitives

### *** Due on 11:59:59pm, June 12 (Friday)***

### Goal
Implement the spinlock and blocking mutex that we learned in the class. And then, build a multithread-safe ring buffer using those lock primitives.

### Problem Specification
- The template code contains so-called lock tester. It will test your lock and ring buffer implementations, which are the goal of this project. The tester provides comprehensive options to test your implemntation in various ways. Try run `./lock` with the `-h` option for the list of options.

- The tester running with `-l` verifies the correctness of your spinlock implementation. Use `-m` option to test your blocking mutex implementation.
  ```
	$ ./lock -l
	$ ./lock -m
	```

- To pass the tester you need to complete the data structures and functions in `pa3.c`. The tester will call `init_*`, `acquire_*`, and `release_*` functions as they are needed. You can modify `struct spinlock` and `struct mutex` to implement what you want to do.

- The correctness testing will take a while, but is supposed to be finished within a minute. Taking longer than this time bound implies two possibilities;
  - Your implementation ~~sucks~~ performs too badly. You need to improve your implementation to pass the test.
  - Blocked indefinitely. Your implementation has some race conditions, making deadlocks and/or starvation. This should be fixed for sure.

- The tester may generate an assertion failure. This also implies that your implementation fails to provide the expected property of the lock primitive.

- Use `compare_and_swap()` in `atomic.h` to implement the spinlock. It is the atomic instruction that we discussed in the class.

- To implement the blocking mutex, you should design a mechanism that puts a calling thread into sleep and wakes up one of waiting threads. Read the comments in the `pa3.c` carefully for your implementation. Also, you may use `struct thread` as is or after modification.
- When multiple threads try to acquire the blocking mutex that is already taken by some other thread, the mutex acquisition requests should be handled in FCFS order. Note that it doesn't matter for spinlock to keep the order, though.
- You may [eat your own dog food](https://en.wikipedia.org/wiki/Eating_your_own_dog_food); you may, but are not restricted to, use your spinlock implementation to keep the waiting threads in a list. 
- The tester determines whether your implementation is busy-waiting or blocking. In the most cases, it will give you correct results. However, if you are sure that it keeps misjudging your implementation, ask the instructor via email.

- When you run the tester with `-r` option, it tests your ring buffer implementation. Basically, the tester simulates *the n-producer 1-consumer situation* that is discussed in the class. The producers (i.e. generators) generate integer numbers and enqueue them into the ring buffer by calling `enqueue_into_ringbuffer()`. The consumer (i.e., counter) counts the numbers by getting numbers from the ring buffer by calling `dequeue_from_ringbuffer()`. Your implementation will pass the test if and only if the numbers the generators generate match to the numbers the counter counts. Mismatching of the numbers indicates that your implementation fails to synchronize the generators and counter.

- To implement the ring buffer, you can modify `struct ringbuffer` as you need, but you should use `slots` and `nr_slots` **as is**; you should use `slots` to buffer values. Also do not try to replace this pointer variable with an array, or allocate your own buffer to store values. Your points will be revoked if you don't follow this restriction.

- You can eat as much dog food as you want to implement the ring buffer ;-) Either/both spinlock and/or blocking mutex will be OK as long as your ring buffer runs correctly.

- You may get bonus points by completing the following challenges.
  - Implement a **blocking counting** semaphore, and realize the mutex by initializing the semaphore with S=1.
  - Actualize the ring buffer using the counting and binary semaphores as illustrated on page 8 of the lecture material #16.
![Ring buffer](https://sslab.ajou.ac.kr/attend/pa3-ringbuffer.png)


### Restriction and tips
- Following cases can be happened if your implementation has a race condition. This means your implementation is **WRONG**, thereby should be fixed to get the points. Thus, questions regarding these situation will not get any help from the instructor.
  - Testing fails *SOMETIMES*
  - Testing works *SOMETIMES*
  - Some features are working but some features are not
  - Testing works on your computer but not on the server
  - Testing works on the server but not on your computer
  - Testing works on a set of options but not on a different set of options.
  - Testing works when you attach a debugger to the process, but does not without the debugger.

- Due to those uncertainty, confirm that your implementation *successfully runs on the server three times in a row*. You may get reduced points when your submission fails during the grading but there is no three consecutive successes in your submission history. To get guaranteed, submit your final code three times when you are ready.

- While doing the debugging, you may set `nr_testers` in `tester.c` to 1 to figure out the overall flow of the framework. The server will evaluate your implementation with (at least) 4 testers, though.

- Do not use any `pthread_mutex_*` nor `sem_*` functions except for those listed below.
  - `pthread_self`
  - `pthread_kill`
  - `pthread_sigmask`
  - `pthread_sigqueue`

- `futex` is also prohibited since it makes blocking mutex implementation trivial. However, the man page for `futex` contains useful information to implement blocking mutex and you may fumble with `futex` for your coding practice.

- Install `glibc-doc` package if your system fails to find the man pages for `pthread_*` 


### Submission / Grading

- Use [PAsubmit](https://sslab.ajou.ac.kr/pasubmit) for submission
  - 450 points + (100 + 10) bonus points

- Code: `pa3.c` (400 pts + 100 bonus pts in total)
	- Spinlock: 50 pts
	- Blocking mutex: 250 pts
	- Ring buffer: 100 pts
	- 100 bonus points.
	  - 70 bouns points if you implement a blocking counting semaphore, and use it to implement the mutex.
	  - 30 bouns points if you implement the ring buffer using your semaphore.

- Document: One PDF document (50 pts). It should include followings;
  - How the spinlock is implemented.
  - How the blocking mutex is implemented. Specify the thread blocking and waking up mechanisms.
  - Lessons learned (if you have any)
  - No more than three pages; otherwise you will get 0 pts for the report
	- You should explicitly mention your bonus implementation to get considered for the bonus points. You may write more than the three-page limit if you need space to explain your bonus implementation.

- Git repository URL at git.ajou.ac.kr (10 bonus pts)
	- Register http URL and with a deploy token and password.
	- Start the repository by cloning this repository.
	- Make sure the token is valid through June 17 (due + 4 slip days + 1 day)

- *THE INSTRUCTOR WILL NOT ANSWER THE QUESTIONS ABOUT THOSE ALREADY SPECIFIED ON THE HANDOUT.*
- *QUESTIONS OVER EMAIL WILL BE IGNORED UNLESS IT CONCERNS YOUR PRIVACY.*
