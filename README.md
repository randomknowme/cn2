https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3

gpt https://chatgpt.com/c/677ca69b-2d08-8001-be39-58ac8e4fb8fd

# Dining Philosophers Using Monitors
The Reader-Writer Problem ensures multiple readers can read at the same time, but only one writer can write to prevent data inconsistency.
Uses synchronization mechanisms to avoid race conditions and starvation.
```c
Initialize mutex
For each philosopher i:
    Initialize condition[i]
    Set state[i] = THINKING

Function test(i):
    If (state[i] == HUNGRY AND state[left(i)] != EATING AND state[right(i)] != EATING):
        state[i] = EATING
        Signal condition[i] // Allow philosopher i to eat

Function take_forks(i):
    Lock mutex
    state[i] = HUNGRY
    test(i)
    While (state[i] != EATING):
        Wait on condition[i]
    Unlock mutex

Function put_forks(i):
    Lock mutex
    state[i] = THINKING
    test(left(i))
    test(right(i))
    Unlock mutex

Function philosopher(i):
    While true:
        THINK
        take_forks(i)
        EAT
        put_forks(i)

Start N philosopher threads running philosopher(i)
Wait for all philosophers to finish

```
--- 
# Pseudocode for Reader-Writer Problem Using Monitors
A monitor-based solution where readers and writers use condition variables and mutex locks for controlled access.
Monitors automatically handle synchronization without explicit locks, ensuring exclusive writing and simultaneous reading.
```c
Initialize:
    int read_count = 0    // Count of active readers
    boolean writing = false  // True if a writer is writing
    Mutex mutex       // Protects read_count
    Condition can_read, can_write // Synchronization conditions

Function start_reading():
    Lock mutex
    While (writing):  // Wait if a writer is active
        Wait(can_read, mutex)
    read_count += 1
    Signal(can_read)  // Allow more readers if no writer is waiting
    Unlock mutex

Function stop_reading():
    Lock mutex
    read_count -= 1
    If (read_count == 0):  // Last reader unlocks writer
        Signal(can_write)
    Unlock mutex

Function start_writing():
    Lock mutex
    While (writing OR read_count > 0): // Wait if a writer is active or readers exist
        Wait(can_write, mutex)
    writing = true
    Unlock mutex

Function stop_writing():
    Lock mutex
    writing = false
    Signal(can_write)  // Allow next writer if any
    Signal(can_read)   // Allow readers to proceed
    Unlock mutex

Function Reader():
    While (true):
        start_reading()
        READ DATA
        stop_reading()

Function Writer():
    While (true):
        start_writing()
        WRITE DATA
        stop_writing()

```
---
# Pseudocode for Reader-Writer Problem Without Monitors
Implements explicit semaphores or mutexes to control reader and writer access.
First reader locks writers, last reader unlocks writers, and each writer gets exclusive access.
```c
Function Reader():
    Wait(mutex)  
    read_count += 1  
    If (read_count == 1):  
        Wait(rw_mutex)  // First reader blocks writers
    Signal(mutex)  

    PRINT "Reader is reading"
    SLEEP(1)  // Simulate reading time

    Wait(mutex)  
    read_count -= 1  
    If (read_count == 0):  
        Signal(rw_mutex)  // Last reader allows writers
    Signal(mutex)

Function Writer():
    Wait(rw_mutex)  // Only one writer at a time
    PRINT "Writer is writing"
    SLEEP(2)  // Simulate writing time
    Signal(rw_mutex)  // Allow others to proceed

Function Main():
    Initialize mutex = 1
    Initialize rw_mutex = 1
    Create N reader threads calling Reader()
    Create M writer threads calling Writer()
    Wait for all threads to complete

```
# Producer-Consumer Problem
The Producer-Consumer Problem is a classic synchronization problem where a producer generates data and places it in a shared buffer, while a consumer removes and processes the data from the buffer. This scenario requires proper synchronization to prevent issues like race conditions, buffer overflow, or buffer underflow. The problem is often solved using semaphores or mutex locks to ensure mutual exclusion and correct access to the shared buffer.

```c
Initialize:
    Semaphore empty = N   // Number of empty slots in the buffer
    Semaphore full = 0    // Number of filled slots in the buffer
    Semaphore mutex = 1   // Mutual exclusion for buffer access

Function Producer():
    While true:
        Produce item
        Wait(empty)       // Wait if buffer is full
        Wait(mutex)       // Enter critical section
        Add item to buffer
        Signal(mutex)     // Exit critical section
        Signal(full)      // Increase count of filled slots

Function Consumer():
    While true:
        Wait(full)        // Wait if buffer is empty
        Wait(mutex)       // Enter critical section
        Remove item from buffer
        Signal(mutex)     // Exit critical section
        Signal(empty)     // Increase count of empty slots
        Consume item

Main:
    Create Producer and Consumer threads
    Run both concurrently
```

https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3
