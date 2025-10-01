---
content_type: page
description: This contains the instructions and questions for the Raft assignment
  on "In Search of an Understandable Consensus Algorithm" by Diego Ongaro and John
  Ousterhout.
hide_download: true
hide_download_original: null
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 11: Security Part I'
parent_type: CourseSection
parent_uid: c7a234fb-b37e-e20a-41b9-581882a0afcd
title: Raft Assignment
uid: 664be575-c00e-8b73-1e97-37cd0a323c01
---

For this recitation, you'll be reading "[In Search of an Understandable Consensus Algorithm (PDF)](https://raft.github.io/raft.pdf)" by Diego Ongaro and John Ousterhout. This paper describes Raft, an algorithm for achieving distributed consensus. The paper contrasts Raft to an algorithm called Paxos: You do **not** need to know anything about Paxos to read this paper. Raft was designed to be more understandable than Paxos.

Before reading the paper, check out two very helpful websites, which have some useful visualizations:

*   An [introduction](http://thesecretlivesofdata.com/raft/) to distributed consensus
*   A [visualization](https://raft.github.io/) of Raft

With those visualizations in mind, read the paper. Skip sections 5.4.3 and 7, and skim sections 9.1 and 9.2.

*   The first four sections give background and motivation for Raft. Sections five and six are the primary technical sections.
*   Fig. 2 is a good reference to come back to after you've read the paper. Don't get stuck trying to memorize the entire table before you move onto page 5 of the paper; skip it, come back to it during your reading or at the end.

To check your understanding after reading:

*   How does Raft handle the following three types of failures?: Leader failures, candidate or follower failures, and network partitions (a network partition means that one part of the cluster is unable to communicate with any machine in the other part).
*   Take a look at figure 7. For each log, (a)-(f), what sequence of events might have led to that log?

Questions for Recitation
------------------------

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question). 

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   In Raft, what is the leader's function?
*   How does the leader work?
*   Why does Raft need a leader?

As always, there are multiple correct answers for each of these questions.