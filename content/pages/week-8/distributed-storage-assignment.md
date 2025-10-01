---
content_type: page
description: This contains the instructions and questions for the distributed storage
  assignment on the Google File System.
hide_download: true
hide_download_original: null
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 8: Distributed Systems Part I'
parent_type: CourseSection
parent_uid: 03839826-8d83-1a70-6fad-0af0bfa301d7
title: Distributed Storage Assignment
uid: 766d3b59-5f03-a2f4-b1ad-4d15d2427648
---

Read "[The Google File System](https://ai.google/research/pubs/pub51)" by S. Ghemawat, H. Gobioff & S-T Leung. You've seen GFS before: It is the system that MapReduce relied on to replicate files.

GFS is a system that replicates files across machines. It's meant for an environment where lots of users are writing to the files, the files are really big, and failures are common. Section 2–4 of the paper describe the design of GFS, Section 5 discusses how GFS handles failures, and Sections 6–7 detail their evaluation and real-world usage of GFS.

To check whether you understand the design of GFS, you should be able to answer the following questions: What is the role of the master? How does a read work? How does a write work?

As you read, think about:

*   Why does GFS use a large chunk size?
*   What happens if a replica in GFS fails?
*   What happens if the master fails?

Questions for Recitation
------------------------

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question). 

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   What assumptions does GFS rely on?
*   How does it exploit those assumptions?
*   Why does GFS make those assumptions?

As always, there are multiple correct answers for each of these questions.