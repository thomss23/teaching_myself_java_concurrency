- Threads are sometimes called lightweight processes. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.

- Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.

- An interrupt is an indication to a thread that it should stop what it is doing and do something else. It's up to the programmer to decide exactly how a thread responds to an interrupt, but it is very common for the thread to terminate. This is the usage emphasized in this lesson.


## Interuptions
How does a thread support its own interruption? This depends on what it's currently doing. If the thread is frequently invoking methods that throw InterruptedException, it simply returns from the run method after it catches that exception. For example, suppose the central message loop in the SleepMessages example were in the run method of a thread's Runnable object. Then it might be modified as follows to support interrupts:

for (int i = 0; i < importantInfo.length; i++) {
// Pause for 4 seconds
try {
Thread.sleep(4000);
} catch (InterruptedException e) {
// We've been interrupted: no more messages.
return;
}
// Print a message
System.out.println(importantInfo[i]);
}
Many methods that throw InterruptedException, such as sleep, are designed to cancel their current operation and return immediately when an interrupt is received.

What if a thread goes a long time without invoking a method that throws InterruptedException? Then it must periodically invoke Thread.interrupted, which returns true if an interrupt has been received. For example:

for (int i = 0; i < inputs.length; i++) {
heavyCrunch(inputs[i]);
if (Thread.interrupted()) {
// We've been interrupted: no more crunching.
return;
}
}

## Joins
The join method allows one thread to wait for the completion of another. If t is a Thread object whose thread is currently executing,

t.join();


## Synchronization
- threads communicate primarily by sharing access to fields and the objects reference fields refer to.
- 2 kinds of errors can appear out of this: thread interference and memory consistency errors. We can prevent this using syncronization.


## Synchronization

- When a thread calls a synchronized method on an object, it acquires a lock (or monitor) associated with that object.