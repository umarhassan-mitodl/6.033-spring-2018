---
content_type: page
description: This contains the content outline for lecture 16.
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 9: Distributed Systems Part II'
parent_type: CourseSection
parent_uid: aa415ef7-5752-19ee-a10a-fb9dc2dbef65
title: Lecture 16 Outline
uid: c46fe11e-d023-04f5-7535-4315faa42e52
---

1.  Introduction
    *   Currently: Building reliable systems out of unreliable components. We're working on implementing transactions which provide:
        *   Atomicity
        *   Isolation
    *   So far: Have a poorly-performing version of atomicity via shadow copies.
    *   Today: Logging, which will give us reasonable performance for atomicity. Logging also works when we have multiple concurrent transactions, even though for today we're not thinking about concurrency.
2.  Motivating Example
    *   {{< tableopen >}}
        {{< tropen >}}
        {{< tdopen >}}
        begin
        {{< tdclose >}}
        {{< tdopen >}}
        // T1
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        A = 100
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        B = 50
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        commit
        {{< tdclose >}}
        {{< tdopen >}}
        // At commit: A=100; B=50
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        begin
        {{< tdclose >}}
        {{< tdopen >}}
        // T2
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        A = A - 20
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        B = B + 20
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        commit
        {{< tdclose >}}
        {{< tdopen >}}
        // At commit: A=80; B=70
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        begin
        {{< tdclose >}}
        {{< tdopen >}}
        // T3
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        A = A + 30
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        {{< tropen >}}
        {{< tdopen >}}
        —CRASH—
        {{< tdclose >}}
        {{< tdopen >}}
         
        {{< tdclose >}}
        
        {{< trclose >}}
        
        {{< tableclose >}}
        
    *   Problem: A = 110, but T3 didn't commit. We need to revert.
3.  Basic Idea
    *   Keep a log of all changes and whether a transaction commits or aborts.
        *   Every transaction gets a unique ID.
        *   UPDATE records include old an new values of a variable.
        *   COMMIT records specify that transaction committed..
        *   ABORT records specify that transaction aborted.
            *   Not always needed.
    *   (See {{% resource_link 76fa2168-e5a4-c472-2c31-5a84b8e09a8c "Lecture 16 slides (PDF)" %}} for the log for this example.)
    *   Nice: Updates are small appends.
4.  How to Use a Log for Transactions
    *   On begin: Allocate new transaction ID (TID).
    *   On write: Append entry to log.
    *   On read: Scan log to find last committed value.
    *   On commit: Write commit record.
        *   This is the commit point.
        *   Atomic because we can assume it's a single-sector write.
        *   Another way to do it would be to put checksums on each record and ignore partially-written records.
    *   On abort: Nothing (could write an ABORT record but not strictly needed).
    *   On recover: Nothing.
    *   (see {{% resource_link 76fa2168-e5a4-c472-2c31-5a84b8e09a8c "Lecture 16 slides (PDF)" %}} for code.)
5.  Performance of Log
    *   Writes: Good. Sequential = fast.
    *   Reads: Terrible. Must scan entire log.
    *   Recovery: Instantaneous.
6.  Cell Storage
    *   Improve read performance with cell storage.
        *   (For us) stored on disk, i.e., non-volatile storage.
        *   Updates go to log and cell storage.
        *   Read from cell storage.
    *   "Log" = write to log. "Install" = write to cell storage.
    *   How to recover:
        *   Scan the log backwards, determine what actions aborted, and undo them.
        *   (see {{% resource_link 76fa2168-e5a4-c472-2c31-5a84b8e09a8c "Lecture 16 slides (PDF)" %}} for code.)
        *   What if we crash during recovery? No worries; recover() is idempotent. Can do it repeatedly.
    *   How to write:
        *   Log before install, not the other way; otherwise, can't recover from a crash in between the two writes.
        *   This is write-ahead logging.
7.  Performance of Log + Cell Storage
    *   Writes: Okay, but now we write to disk twice instead of once.
    *   Reads: Fast.
    *   Recovery: Bad. Have to scan the entire log.
8.  Improving Performance
    *   Improve writes: Use a (volatile) cache.
        *   Reads go to cache first, writes go to cache and are eventually flushed to cell storage.
        *   Problem: After crash, there may be updates that didn't make it to cell storage (were in cache but not flushed).
            *   Also could be updates in cell storage that need to be undone, but we had that problem before.
        *   Solution: We need a redo phase in addition to an undo phase in our recovery.
    *   Improving recovery:
        *   Problem: Recovery takes longer and longer as the log grows.
        *   Solution: Truncate the log.
        *   How?
            *   Assuming no pending actions:
                *   Flush all cached updates to cell storage.
                *   Write a CHECKPOINT record.
                *   Truncate the log prior to the CHECKPOINT record.
                    *   Usually amounts to deleting a file.
            *   With pending actions, delete before the checkpoint and earliest undecided record.
    *   ABORT records
        *   Can be used to help recovery and skip undo-ing aborted transaction. Not necessary for correctness—can always just pretend we crashed—but can help.
9.  What about Un-undo-able Actions?
    *   What if our transaction fires a missile and then aborts?
    *   Typically: Wait for software that controls the action to commit and then take the action, but have a special way to detect whether the action has/will happened.
10.  Summary
    *   Logging is a general technique for achieving atomicity.
        *   Writes are fast, reads can be fast with cell storage.
        *   Need to log before installing (write-ahead), and need a recovery process.
    *   Tomorrow is recitation: Logging for file systems.
    *   Now: We're good with atomicity.
        *   In fact, logging will work fine with concurrent transactions; the problem will be figuring out which steps we can actually run in parallel.
    *   Wednesday: Isolation.
    *   Next week: Distributed transactions.