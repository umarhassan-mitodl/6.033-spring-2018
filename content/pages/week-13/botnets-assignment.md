---
content_type: page
description: 'This contains the instructions and questions for the assignment on "Your
  Botnet is My Botnet: Analysis of a Botnet Takeover " by Stone-Gross, et al.'
draft: false
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 13: Security Part III'
parent_type: CourseSection
parent_uid: 87aba984-30c8-d18b-3f71-7bdec998328f
title: Botnets Assignment
uid: ea13cbf1-25e6-cb4f-d552-53fee6563f83
---
Read "[Your Botnet is My Botnet: Analysis of a Botnet Takeover](https://dl.acm.org/doi/10.1145/1653662.1653738)" by Stone-Gross, et al. You can skim sections 5.2.1–5.2.3.

- Section 2 explains how Torpig infects a user's machine. After reading this section, you should understand how that happens: how the rootkit gets installed and why its installation remains undetected.
- Section 3 explains the technique (domain flux) by which Torpig bots communicate with the C&C (command and control) server. After reading this section, you should understand why it's difficult to simply block bots from accessing the C&C server.
- Section 4 explains how the authors were able to take control of the Torpig botnet for a few weeks.
- Sections 5 and 6 give an analysis of the botnet based on the authors' takeover. (Remember you can skim sections 5.2.1–5.2.3)

As you read, think about

- What tools and/or threat models are violated by Torpig?
- Think about the key factors that allowed the authors to infiltrate the Torpig botnet. Would their techniques work for all botnets?
- Was the authors' methodology—taking over the botnet—necessary to collect the data they wanted? Was it ethical?

## Questions for Recitation

**Before you come to this recitation**, write up (on paper) a *brief* answer to the following (really—we don't need more than a couple sentences for each question).  

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

- What does it mean for a computer to be a "bot"?
- In Torpig, how does a machine become a bot, and how does it receive instructions to carry out attacks?
- Why is it difficult to prevent a machine from becoming a bot? Why is it difficult to simply block bots from accessing the C&C server?

As always, there are multiple answers to each of these questions.