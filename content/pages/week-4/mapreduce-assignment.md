---
content_type: page
description: This contains the instructions and questions for the MapReduce assignment.
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 4: Operating Systems Part IV'
parent_type: CourseSection
parent_uid: 0466ee2b-5ebb-72d0-ad4f-7badf3b6c645
title: MapReduce Assignment
uid: 669830ff-a145-23d6-dcee-00927f0c1d39
---

Preparation for MapReduce recitation

*   Read "[MapReduce (PDF)](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)" by J. Dean & S. Ghemawat.
*   Skip sections 4 and 7

This paper was published at the biennial Usenix Symposium on Operating Systems Design and Implementation (OSDI) in 2004, one of the premier conferences in computer systems. (OSDI alternates with the equally prestigious ACM Symposium on Operating Systems Principles (SOSP), at which appeared Eraser, the paper you already read in a previous recitation.)

After reading through Section 3, you should be able to understand and explain Figure 1 (the "Execution overview"). After reading Sections 5 and 6, you should understand the real-world performance of MapReduce. An example question that you should be able to answer: How do stragglers effect performance?

As you read, think about the following:

*   MapReduce has a constrained programming model. Are the benefits of using MapReduce worth that constraint?
*   What types of failures does MapReduce handle, and how does it handle them?

Question for Recitation
-----------------------

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question). 

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   What are the performance goals of MapReduce (both the programming model + its implementation)?
*   How was MapReduce implemented at Google to meet those goals?
*   Why was MapReduce implemented in this way?

As always, there are multiple correct answers for each of these questions.