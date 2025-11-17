# Chapter 1 - Fundamentals 

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
- 2 kinds of errors can appear out of this: thread interference and memory consistency errors. We can prevent this using syncronization
- When a thread calls a synchronized method on an object, it acquires a lock (or monitor) associated with that object.

### Atomic Access

- An atomic operation means that an action happens effectively all at once. It cannot stop in the middle, it either happens completely or it doesn't happen at all.
- Reads and writes are atomic for reference variables and for most primitive variables (all types except long and double).
- Reads and writes are atomic for all variables declared volatile (including long and double variables).

### Liveness

- Def: a concurrent app's ability to execute in a timely manner
- Problems: deadlock, starvation and livelock

### Deadlock

- when two or more threads are blocked forever, waiting for each other

### Starvation

- a thread is unable to gain regular access to shared resrouces and is unable to make progress. For example, suppose an object provides 
a synchronized method that often takes a long time to return. If one thread invokes this method frequently, 
other threads that also need frequent synchronized access to the same object will often be blocked

### Livelock

- A thread often acts in response to the action of another thread. 
If the other thread's action is also a response to the action of another thread, then livelock may result. 


## Guarded Blocks

- Threads often have to coordinate their actions. The most common coordination idiom is the guarded block. 

Suppose, for example guardedJoy is a method that must not proceed until a shared variable joy has been set by another thread. Such a method could, in theory, simply loop until the condition is satisfied, but that loop is wasteful, since it executes continuously while waiting.

public void guardedJoy() {
// Simple loop guard. Wastes
// processor time. Don't do this!
while(!joy) {}
System.out.println("Joy has been achieved!");
}
A more efficient guard invokes Object.wait to suspend the current thread. The invocation of wait does not return until another thread has issued a notification that some special event may have occurred — though not necessarily the event this thread is waiting for:

public synchronized void guardedJoy() {
// This guard only loops once for each special event, which may not
// be the event we're waiting for.
while(!joy) {
try {
wait();
} catch (InterruptedException e) {}
}
System.out.println("Joy and efficiency have been achieved!");
}

## Immutable Objects
- An object is considered immutable if its state cannot change after it is constructed. Maximum reliance on immutable objects is widely accepted as a sound strategy for creating simple, reliable code.