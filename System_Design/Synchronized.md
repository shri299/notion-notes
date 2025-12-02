## 🔐 What is synchronized?
synchronized is a keyword that ensures:
  - Only one thread at a time can execute a block or method protected by a monitor lock
  - It also ensures visibility: changes made by one thread inside the lock are visible to others after the lock is released
## 🔧 Syntax Variants
### 1. Synchronized Method
```java
java
public synchronized void doWork() {
    // synchronized on `this`
}
```
### 2. Synchronized Block
```java
java
public void doWork() {
    synchronized (this) {
        // critical section
    }
}
```
### 3. Static Synchronized Method
```java
java
public static synchronized void doStaticWork() {
    // synchronized on Class object
}
```
## 🧠 What Does synchronized Guarantee?
## 🔁 How It Works Internally
### Behind the scenes:
- JVM uses monitorenter (when entering synchronized) and monitorexit (when exiting)
- These act as memory fences:
  - Before entering, thread flushes its cache to read fresh values
  - On exit, it flushes writes to main memory
This ensures visibility + ordering, just like volatile — but also mutual exclusion
## 🧪 Classic Example: Thread-safe Counter
```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++; // atomic now
    }

    public synchronized int getCount() {
        return count;
    }
}
```
Multiple threads can now safely call increment() and getCount() — no lost updates, no stale reads.
## 💥 Happens-Before in synchronized
Java guarantees that:
✅ An unlock (monitorexit) in one thread happens-before a lock (monitorenter) in another thread on the same monitor
So any thread that acquires the lock is guaranteed to:
- See all updates made by previous threads that released the same lock
## 🔄 Comparison with volatile and AtomicInteger
## ⚠️ Downsides of synchronized
## ✅ When to Use synchronized
Use it when:
- You need atomicity + visibility across multiple operations
- You're modifying multiple shared variables
- You need to ensure consistency, not just fast updates


## 👇 Scenario Recap
```java
java
CopyEdit
class MyClass {
    public synchronized void m1() {
        // some work
    }

    public synchronized void m2() {
        // some work
    }
}
```
Now:
```java
java
CopyEdit
MyClass obj = new MyClass();

Thread t1 = new Thread(() -> obj.m1());
Thread t2 = new Thread(() -> obj.m2());
```
## ❓ Will t2 be able to enter m2() while t1 is in m1()?
❌ NO, t2 will have to wait — because both m1() and m2() are synchronized on the same object monitor (obj)
## 🔐 Explanation:
### 🔸 synchronized methods implicitly lock on this (the object instance)
So:
- m1() is equivalent to synchronized (this) { ... }
- m2() is also synchronized (this) { ... }
### 🔸 Only one thread at a time can hold the lock on this
So when:
- t1 enters obj.m1(), it acquires the monitor lock on obj
- t2 then tries to enter obj.m2(), but it also needs the same lock on obj
- So t2 gets blocked until t1 exits m1() and releases the lock

## ✅ How to Allow Parallel Execution?
If you want t1 and t2 to execute simultaneously, you'd need:
1. Remove synchronized from at least one method, OR
1. Synchronize on different locks
Example with different locks:
```java
java
CopyEdit
class MyClass {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void m1() {
        synchronized (lock1) {
            // work
        }
    }

    public void m2() {
        synchronized (lock2) {
            // work
        }
    }
}
```
Now t1 and t2 can execute m1() and m2() independently 🚀