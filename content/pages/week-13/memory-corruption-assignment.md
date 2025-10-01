---
content_type: page
description: 'This contains the instructions and questions for the memory corruption
  assignment on  "SoK: Eternal War in Memory" by Lazlo Szekeres, Mathia Payer, Tao
  Wei, and Dawn Song.'
hide_download: true
hide_download_original: null
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 13: Security Part III'
parent_type: CourseSection
parent_uid: 87aba984-30c8-d18b-3f71-7bdec998328f
title: Memory Corruption Assignment
uid: f16c7835-ea51-9931-2f17-c5652cc47102
---

For this recitation, you'll be reading "[SoK: Eternal War in Memory (PDF)](https://people.eecs.berkeley.edu/~dawnsong/papers/Oakland13-SoK-CR.pdf)" by Lazlo Szekeres, Mathia Payer, Tao Wei, and Dawn Song. This paper describes a variety of memory corruption bugs, and potential solutions. Don't worry about memorizing every single type of attack described in this paper; aim to understand what makes these attacks possible, and the general ideas behind the solutions.

(We've also written a {{% resource_link 82ef662a-803f-2c31-0c03-cc06053da27b "quick guide (PDF)" %}} to some of the memory corruption bugs described in the paper, which you might want to take a look at before you start reading.)

The paper is fairly well organized:

*   Section I gives you an introduction to the general problem, and a brief history of memory corruption attacks and defenses.
*   Section II details many different types of memory corruption attacks. Figure 1 is particularly helpful in organizing these attacks and their defenses. As stated above, don't worry about memorizing every single type of attack described in this section; aim to understand what makes the various attacks different from one another, as well as what makes each of these possible in the first place.
*   Section III describes two currently-deployed defenses, and their drawbacks.
*   Section IV outlines some properties and requirements for defenses against memory corruption bugs; those defenses are discussed in Sections V-VIII (each section is a different category of defense).
*   Sections IX and X conclude.

As you read, think about the following:

*   Stack canaries (or cookies) and ASLR are both widely-deployed. What is good about those solutions? What attacks do they prevent? What attacks _don't_ they prevent?
*   Many of the proposed solutions worsen performance. Are we at a point where these performance hits are outweighed by security concerns?

Questions for Recitation
------------------------

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question).  

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   What do the authors mean by "the eternal war in memory"?
*   How has this war progressed over time? (i.e., how have attacks and solutions evolved)
*   Why hasn't the war ended?

As always, there are multiple correct answers for each of these questions.