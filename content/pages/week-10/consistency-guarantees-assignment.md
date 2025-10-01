---
content_type: page
description: 'This contains the instructions and questions for the consistency guarantees
  assignment on the "Replicated Data Consistency Explained Through Baseball" paper
  by Doug Terry.    '
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 10: Distributed Systems Part III'
parent_type: CourseSection
parent_uid: 62a94c78-a524-3c30-60aa-11c27ff81fc7
title: Consistency Guarantees Assignment
uid: c109e0c5-f4f6-9b52-7d96-b0c911fcd579
---

Read the "[Replicated Data Consistency Explained Through Baseball (PDF)](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf)" paper by Doug Terry.

This paper is relatively light on technical details, but should give you an understanding of different consistency guarantees and how a system chooses which guarantee to use. Section 2 explains some different guarantees, and Sections 3 and 4 walk through some examples that illustrate places where each guarantee might be used.

Pay close attention to Table 2, which rates each type of guarantee in terms of consistency, performance, and availability. Though the paper doesn't give the technical details that lead to these ratings, you should be able to reason about them based on what you've seen in lecture.

As you read, think about the following:

*   What are other systems where some of the weaker consistency guarantees are applicable?
*   What type of app-specific knowledge matters when determining the "right" consistency guarantee?

Questions for Recitation
------------------------

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question).  

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   What is a consistency guarantee? What aspects of a system does it affect?
*   How does a system designer choose an appropriate consistency guarantee?
*   Why does the choice of consistency guarantee matter?

As always, there are multiple correct answers for each of these questions.