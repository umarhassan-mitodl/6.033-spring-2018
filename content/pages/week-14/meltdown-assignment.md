---
content_type: page
description: This contains the instructions and questions for the assignment on "Meltdown"
  by Moritz Lipp, Michael Schwarz, et al.
hide_download: true
hide_download_original: null
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 14: Security Part IV'
parent_type: CourseSection
parent_uid: a53ea92f-4c3d-bec4-3ffa-e7cf73d6eb29
title: Meltdown Assignment
uid: 447da471-034f-b347-4c11-bbe7bfa9e6b3
---

For this recitation, you'll be reading "[Meltdown (PDF)](https://meltdownattack.com/meltdown.pdf)" by Moritz Lipp, Michael Schwarz, et al. Meltdown, along with Spectre, is a security vulnerability that was discovered early this year, which affects all modern Intel processors.

To help as you read:

*   Sections 2 and 3 give a very good overview of the necessary background and a toy example to help you understand the basic attack.
*   Sections 4 and 5 extend that toy example, explaining how Meltdown was actually implemented.
*   Section 6 evaluates the attack, explaining what systems are vulnerable and how well the attack performs.
*   Sections 7 and 8 discuss countermeasures and some of the consequences of Meltdown.

As you read, think about the following:

*   How does Meltdown differ from the other attacks we've seen? In particular, how does it compare to the memory corruption attacks in the previous recitation?
*   Think about Meltdown in the context of the guard model. Is there a guard in place here? If so, how is it being subverted?
*   The paper (Section 6.4) mentions that ARM and AMD CPUs do not appear susceptible to Meltdown, and posit that it could be that the current implementation of Meltdown is too slow. Why does the speed of the Meltdown code matter here?

Questions for Recitation
------------------------

Think about the following before recitation. You do not need to turn anything in since it's the last week of classes. (Participation **during** this recitation does still count towards your grade.)

*   What is the Meltdown attack?
*   How does it work?
*   Why is this attack possible? (Or an alternative question, why doesn't Intel simply disable out-of-order execution on its processors?)

As always, there are multiple correct answers for each of these questions.