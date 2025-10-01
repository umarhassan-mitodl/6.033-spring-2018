---
content_type: page
description: '  This contains the content outline for lecture 12.'
draft: false
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 7: Networking Part III'
parent_type: CourseSection
parent_uid: 0abfab7f-cd01-9a6a-159d-1afa3fd61f99
title: Lecture 12 Outline
uid: 3ebfda64-abe4-187a-6497-84708a5f0971
---
1. Introduction   
    - Last time: TCP CC. Massive success. Doesn't require us to change the network, is something machines can opt-in to (don't have to have reliable transport if you don't need it), lets us prevent congestion in a distributed manner.
    - But:
        - Can result in long delays when routers have too much buffering.
        - Doesn't work well in some scenarios (DCTCP).
        - Most important for today: Doesn't react to congestion until queues are full.
    - Full queues = long delay.
    - Queues = necessary to absorb bursts.
    - Goal: Transient queues, not persistent queues.
    - Idea: Drop packets \*before\* the queues are full. TCP senders will back off before congestion is too bad.
2. DropTail   
    - The original queue management scheme. When a packet arrives, if the queue is full, drop it; else, enqueue it.
    - Simple (+).
    - Only drops packets when it needs to (+/-).
        - Remember: Dropped packet => retransmission, which wastes resources.
    - Synchronizes sources (-).
    - Not very fair (-).
    - Tends to result in mostly-full queues (-).
    - Bad for bursty traffic (-).
3. RED   
    - Active queue management scheme.
    - Idea: Drop packets before the queue is full to give senders an early signal.
    - Requires a measure of the average queue size, q\_avg.