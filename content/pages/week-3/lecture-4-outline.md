---
content_type: page
description: This contains the content outline for lecture 4.
draft: false
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 3: Operating Systems Part III'
parent_type: CourseSection
parent_uid: 81056cb9-5545-9139-0582-b2e55570d21a
title: Lecture 4 Outline
uid: 4bd4d775-885a-2661-47dc-64a6eba38676
---
1. Previously
    - We're on a quest to enforce modularity on a single machine.
    - Last time: Virtualize memory to prevent programs from accessing each other's memory.
    - This time: Virtualize communication links to allow programs to communicate.
    - Still assuming one program per CPU, and a correct kernel.
2. Bounded Buffers
    - Allow programs to communicate.
    - Another application of virtualization.
    - Stores N messages, to deal with bursts.
    - API: send(m), m \\\\\<- receive()
    - Receivers and senders block if there are no messages (receiver) or no space (sender).
    - Concurrency causes problems in the implementation.
    - Need to decide when it's okay to write, when it's okay to read, and where to write to/read from.
3. Bounded Buffers for Single Senders
    - `send(bb, message): &nbsp;&nbsp;while True: # Wait until it's okay to write &nbsp;&nbsp;&nbsp;if bb.in – bb.out < N: &nbsp;&nbsp;&nbsp;&nbsp;bb.buf[bb.in mod N] <- message &nbsp;&nbsp;&nbsp;&nbsp;bb.in <- bb.in + 1 &nbsp;&nbsp;&nbsp;&nbsp;return`
    - `receive(bb): &nbsp;&nbsp;while True: # Wait until it's okay to read &nbsp;&nbsp;&nbsp;if bb.out < bb.in: &nbsp;&nbsp;&nbsp;&nbsp;message <- bb.buf[bb.out mod N] &nbsp;&nbsp;&nbsp;&nbsp;bb.out <- bb.out + 1 &nbsp;&nbsp;&nbsp;&nbsp;return message`
    - Can't swap the action and the increment; can cause reads of messages that don't exist.
4. Bounded Buffer for Multiple Senders
    - With two senders, different orders of executions will lead to unexpected output in the previous implementation (empty slots in the buffer, too few elements in the buffer).
    - Need locks.
5. Locks
    - Allow only one CPU to be in a piece of code at a time.
    - API: acquire(lock), release(lock)
        - \*Not\* acquire(variable I want to lock)
    - If two CPUs try to acquire the same lock at the same time, one will succeed and the other will block.
6. Bounded Buffers with Locks
    - Attempt 1 (using pseudocode): Locks around every line.
        - `send(int x) { &nbsp;&nbsp;acquire(&lck); &nbsp;&nbsp;buf[in] = x; &nbsp;&nbsp;release(&lck); &nbsp;&nbsp;acquire(&lck); &nbsp;&nbsp;in = in + 1; &nbsp;&nbsp;release(&lck); }`
        - Result: Correct number of elements, but some slots have no messages (A and B write to same slot, and both increment).
    - Attempt 2:
        - `send(int x) { &nbsp;&nbsp;acquire(&lck); &nbsp;&nbsp;buf[in] = x; &nbsp;&nbsp;in = in + 1; &nbsp;&nbsp;release(&lck); }`
        - Correct: We want write and increment to be atomic (happen together).
    - Back to original code. Attempt 1:
        - `send(bb, message): &nbsp;&nbsp;while True: &nbsp;&nbsp;&nbsp;if bb.in — bb.out < N: &nbsp;&nbsp;&nbsp;&nbsp;acquire(bb.lock) &nbsp;&nbsp;&nbsp;&nbsp;bb.buf[bb.in mod N] <- message &nbsp;&nbsp;&nbsp;&nbsp;bb.in <- bb.in + 1 &nbsp;&nbsp;&nbsp;&nbsp;release(bb.lock) &nbsp;&nbsp;&nbsp;&nbsp;return`
        - No: Concurrent senders will both think they can write, the first to acquire the lock might fill up the buffer (and so the second shouldn't write).
    - Attempt 2:
        - `send(bb, message): &nbsp;&nbsp;acquire(bb.lock) &nbsp;&nbsp;while True: &nbsp;&nbsp;&nbsp;&nbsp;if bb.in — bb.out < N: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bb.buf[bb.in mod N] <- message &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bb.in <- bb.in + 1 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;release(bb.lock) &nbsp;&nbsp;&nbsp;&nbsp;return`
        - If the receiver is also trying to acquire lock, this attempt will prevent the receiver from ever receiving (so the sender will keep blocking when the buffer is full). If the receiver is using a different lock we will face issues with concurrently editing the same data structure.
    - Attempt 3 (correct):
        - `send(bb, message): &nbsp;&nbsp;acquire(bb.lock) &nbsp;&nbsp;while bb.in - bb.out = N: &nbsp;&nbsp;&nbsp;&nbsp;release(bb.lock) // repeatedly release and acquire, to allow &nbsp;&nbsp;&nbsp;&nbsp;acquire(bb.lock) // processes calling receive() to jump in &nbsp;&nbsp;bb.buf[bb.in mod N] <- message &nbsp;&nbsp;bb.in <- bb.in + 1 &nbsp;&nbsp;release(bb.lock) &nbsp;&nbsp;return`
7. Atomic Actions
    - How to decide what should make up an atomic action?
        - Too much code in locks: Performance suffers.
        - Too little code in locks: Unexpected behavior.
    - Think of locks as protecting an invariant. Don't release the lock when the invariant is false.
8. Example: Locks for file systems
    - Filesystem move:
        - `move(dir1, dir2, filename): &nbsp;&nbsp;unlink(dir1, filename) &nbsp;&nbsp;link(dir2, filename)`
    - Coarse-grained locking:
        - `move(dir1, dir2, filename): &nbsp;&nbsp;acquire(fs_lock) &nbsp;&nbsp;unlink(dir1, filename) &nbsp;&nbsp;link(dir2, filename) &nbsp;&nbsp;release(fs_lock)`
        - Bad performance: can't move two different files between entirely different directories at the same time.
    - Fine-grained locking:
        - `move(dir1, dir2, filename): &nbsp;&nbsp;acquire(dir1.lock) &nbsp;&nbsp;unlink(dir1, filename) &nbsp;&nbsp;release(dir1.lock) &nbsp;&nbsp;acquire(dir2.lock) &nbsp;&nbsp;link(dir2, filename) &nbsp;&nbsp;release(dir2.lock)`
        - Better performance, but incorrect. What if dir2 is renamed between release and acquire?
        - Bad because CPU sees inconsistent state.
    - Fine-grained locking + holding both locks
        - `move(dir1, dir2, filename): &nbsp;&nbsp;acquire(dir1.lock) &nbsp;&nbsp;acquire(dir2.lock) &nbsp;&nbsp;unlink(dir1, filename) &nbsp;&nbsp;link(dir2, filename) &nbsp;&nbsp;release(dir1.lock) &nbsp;&nbsp;release(dir2.lock)`
        - Deadlock when A does move(M, N, file1.txt), B does move(N, M, file2.txt).
    - Fine-grained locking + solving deadlock
        - Heuristic: Look for all places where multiple locks are held, and ensure that locks are acquired in the same order.
        - `move(dir1, dir2, filename): &nbsp;&nbsp;if dir1.inum < dir2.inum: &nbsp;&nbsp;&nbsp;&nbsp;acquire(dir1.lock) &nbsp;&nbsp;&nbsp;&nbsp;acquire(dir2.lock) &nbsp;&nbsp;else: &nbsp;&nbsp;&nbsp;&nbsp;acquire(dir2.lock) &nbsp;&nbsp;&nbsp;&nbsp;acquire(dir1.lock) &nbsp;&nbsp;unlink(dir1, filename) &nbsp;&nbsp;link(dir2, filename) &nbsp;&nbsp;release(dir1.lock) &nbsp;&nbsp;release(dir2.lock)`
        - Painful: Requires global reasoning about all locks.
    - Answer? Start coarse-grained and refine.
9. Implementing Locks
    - Attempt 1:
        - `acquire(lock): &nbsp;&nbsp;while lock != 0: &nbsp;&nbsp;&nbsp;do nothing &nbsp;&nbsp;lock = 1`
        - `release(lock): &nbsp;&nbsp;lock = 0`
        - Race condition: Both see lock = 0, set lock = 1, and execute code.
    - Problem: Need locks to implement locks.
    - Solution: Hardware support (atomic instructions).
    - x86 example: XCHG
        - `XCHG reg, addr &nbsp;&nbsp;temp <- mem[addr] &nbsp;&nbsp;mem[addr] <- reg &nbsp;&nbsp;reg <- temp`
    - Now:
        - `acquire(lock): &nbsp;&nbsp;do: &nbsp;&nbsp;&nbsp;r <= 1 &nbsp;&nbsp;&nbsp;XCHG r, lock &nbsp;&nbsp;while r == 1`
    - Atomic operations made possible by the controller that manages access to memory.