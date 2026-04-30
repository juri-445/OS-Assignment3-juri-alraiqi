# Assignment 3 - Complete Documentation

**Student Name**: [juri abdulrahim alraiqi]  
**Student ID**: [445051964]  
**Date Submitted**: [28/04/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date, Time]
**What I implemented**: 
Initial project setup, creating the main file structure, and defining basic variables and data structures.
**Challenges encountered**: 
Confusion regarding how to structure the logic for the specific OS algorithm required.
**How I solved it**: 
Reviewed the lecture slides and searched for documentation on standard implementation practices.
**Testing approach**: 
Compiled the code to ensure there are no syntax errors or environment issues.
**Time spent**: 
2 hours
---

### Entry 2 - [Date, Time]
**What I implemented**: 
Developed the core logic of the assignment 
**Challenges encountered**: 
Logical errors where the output values did not match the expected theoretical results.
**How I solved it**: 
 Used print statements to trace the variable values step-by-step and fixed the calculation formulas
**Testing approach**: 
Running the program with basic input sets provided in the assignment description.
**Time spent**: 
 3 hours.
---

### Entry 3 - [Date, Time]
**What I implemented**: 
Refined the code, added comments for clarity, and handled edge cases
**Challenges encountered**: 
The program crashed when dealing with unexpected or large input values
**How I solved it**: 
Added validation checks (if-statements) to ensure the program handles errors gracefully.
**Testing approach**: 
Final end-to-end testing with multiple scenarios to ensure stability
**Time spent**: 
 2 hours
---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?contextswitchcount++.
- Why is concurrent access a problem?interrupts.
- What incorrect behavior could occur?Different data can be stored within switched accounts

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
Semaphores use an integer variable 'S' to control access

Locks use a boolean variable to manage critical sections.
[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
It is a situation where processes are stuck forever because each one is waiting for a resource held by the other.
Two prevention techniques:
1. Lock Ordering: All threads must take locks in the same order.
2. Timeouts: If a thread waits too long, it releases its locks and tries again.
What I did in my code:
I used Lock Ordering to prevent cycles and used try-finally blocks to make sure every lock is always released.
[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
My choice:
I used separate locks for each counter (Fine-grained locking).
Why?
Since the counters are independent, using separate locks allows multiple threads to update different counters at the same time without waiting for each other.
Trade-offs:
• One lock (Coarse): Simple to implement but slow because it blocks all threads.
• Separate locks (Fine): Faster and more efficient but requires more care to avoid deadlocks.
Which provides better concurrency?
The separate locks approach, because it increases performance by allowing threads to work simultaneously on independent tasks.
[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
The shared counter variables: contextSwitchCount, completedProcessCount, and totalWaitingTime.
**Why they need protection**: 
These variables are shared among multiple threads. Without protection, a "Race Condition" could occur where multiple threads try to update the counters at the same time, leading to incorrect or lost data.
**Synchronization mechanism used**: 
I used Fine-grained locking with the ReentrantLock class. Each counter has its own specific lock (contextSwitchLock).
**Code snippet**:
```java
// Paste your implementation here
```
public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}

**Justification**: 
Using ReentrantLock ensures Mutual Exclusion, meaning only one thread can update a specific counter at a time. I chose "Fine-grained" locks (a separate lock for each counter) to improve performance, allowing threads to update different counters simultaneously without blocking each other. The try-finally block ensures the lock is always released even if an error occurs.
---

### Critical Section #2: Execution Log

**What resource**: 
The executionLog which is a List<String> used to store all events during the simulation
**Why it needs protection**: 
In Java, ArrayList is not thread-safe. If multiple processes (threads) try to add messages to the log at the same time, it can lead to data corruption, missing log entries, or a ConcurrentModificationException.
**Synchronization mechanism used**: 
I used a ReentrantLock named logLock to wrap the access to the list.
**Code snippet**:
```java
// Paste your implementation here
```
public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}

**Justification**: 
Using a dedicated logLock ensures that only one thread can modify the executionLog at any given moment (Mutual Exclusion). By using a separate lock for the log instead of a global lock, I maintained the fine-grained locking strategy, which keeps the system efficient by not blocking counter updates while logging is happening.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
The semaphore is used as a binary semaphore (mutex) to control access to the CPU. It ensures that only one process can execute on the CPU at any given time.
**Number of permits and why**: 
1 permit. Because it acts as a binary semaphore to ensure Mutual Exclusion, preventing multiple processes from running simultaneously on the single CPU.
**Where implemented**: 
In the Process class within the run() and runToCompletion() methods
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // Process execution logic here
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 
creates a disciplined execution flow where threads (processes) must wait for the semaphore to be available before starting. This prevents execution overlaps and ensures the simulation reflects a real-world single-core CPU
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```Run the program 5 times with the same inputs and compare the "Average Waiting Time" and "Total Context Switches."

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 
Without synchronization, Race Conditions would occur. For example, two threads might increment contextSwitchCount at the same moment, but only one update would be saved. This leads to inconsistent and wrong statistics across different runs.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
I attempted to add logs to the executionLog from multiple threads simultaneously without using logLock.
**Results**: 
The program threw a ConcurrentModificationException or crashed
**What this proves**: It proves that standard Java collections like ArrayList are not thread-safe and require explicit locking (logLock) to handle concurrent access.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
Verifying that "Total Burst Time" matches the sum of individual process burst times.
**Expected values**: 
Total Burst Time = \sum (\text{Individual Burst Times}).
**Actual values**: 
The values matched perfectly because of the precise locking mechanism.
**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]
Synchronization is essential for data integrity in multi-threaded applications. I learned that without locks, shared variables like counters become unreliable due to race conditions. Using Fine-grained locking (separate locks) is better than one big lock because it allows more threads to work on independent data at the same time, which improves performance. The biggest challenge was ensuring that locks are always released using finally blocks to avoid deadlocks.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Online Banking: To prevent two people from withdrawing the same money from a shared account at the exact same time.

**Example 2**: E-commerce Inventory: To ensure that only one customer can buy the "last item" in stock

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]
Imagine a single-person bathroom in a busy restaurant. The "Lock" on the door is the synchronization. If there was no lock, anyone could walk in at any time (Race Condition), leading to a mess. The lock ensures that only one person uses the resource at a time, and others must wait in a "Ready Queue" until the lock is released.
---

## Part 6: GitHub Repository Information

**Repository URL**: ASSIGNMENT_DOCUMENTATION.md

**Number of commits**: 10

**Commit messages**:
1. t1
2. t2
3. t3
4. t4

---

## Summary

**Total time spent on assignment**: 6h

**Key takeaways**: 
1. Learned how to apply ReentrantLock to protect shared data.
2. Understood the practical difference between coarse-grained and fine-grained locking.
3. Mastered the use of Semaphores to manage resource limits (CPU access

**Most challenging aspect**: 
Designing the fine-grained locking logic to ensure that no race conditions occur while maintaining high execution speed.
**What I'm most proud of**: 
I am most proud that I successfully implemented Fine-Grained Locking and Semaphores to ensure perfect synchronization. This made the simulation highly efficient and data-accurate, which was the most technical part of the assignment.
---

**End of Documentation**
