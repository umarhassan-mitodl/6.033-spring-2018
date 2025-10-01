---
content_type: page
description: This contains the instructions and questions for the assignment on "Security
  Vulnerabilities in DNS and DNSSEC" by Suranjith Ariyapperuma and Chris Mitchell.
hide_download: true
hide_download_original: null
learning_resource_types: []
ocw_type: CourseSection
parent_title: 'Week 12: Security Part II'
parent_type: CourseSection
parent_uid: 463ad0d7-960d-0f16-fac5-ad1eab91ef20
title: DNSSEC Assignment
uid: d7e80479-aaf3-38d9-e79e-b2f538c418b3
---

Read ["Security Vulnerabilities in DNS and DNSSEC (PDF)"](http://www.chrismitchell.net/svidad.pdf) by Suranjith Ariyapperuma and Chris Mitchell. This paper is about DNSSEC. DNS, as is, is an insecure system; DNSSEC is a proposed extension to DNS to mitigate some of the security concerns. It is not yet widespread.

*   Section 2 gives an overview of DNS. Read it if you need a refresher on the protocol, but if not, you can skip it.
*   Section 3 details some of the vulnerabilities to which DNS is open.
*   Section 4 describes DNSSEC, which addresses some of the vulnerabilities in Section 3. DNSSEC has its own problems, however, which are detailed in Section 5.

As you read, think about

*   What are the consequences for users (such as yourself) of the vulnerabilities of DNS?
*   Why must DNSSEC be backwards-compatible with DNS?
*   Why are chains of trust necessary?
*   Who should be in charge of the root key?

##### Questions for Recitation

**Before you come to this recitation**, write up (on paper) a _brief_ answer to the following (really—we don't need more than a couple sentences for each question). 

Your answers to these questions should be **in your own words**, not direct quotations from the paper.

*   From a security standpoint, what does DNSSEC provide? (e.g., confidentially, authentication, etc.)
*   How does it provide that?
*   Why is DNSSEC necessary (or _is_ it necessary?), and why hasn't it been fully deployed?

As always, there are multiple answers to each of these question