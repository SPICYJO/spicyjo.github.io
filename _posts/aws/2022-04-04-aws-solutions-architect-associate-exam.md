---
layout: post
title: Passed AWS Certified Solutions Architect Associate exam! 
description: I took AWS Certified Solutions Architect Associate exam today. I am going to share some information about the exam and my study tips.
author: Seungwoo Jo
# last_modified_at: 2022-01-01 00:00:00 +0900
# math: false
tags: aws, certification
category: aws
comments: true
---

Today, I took an AWS Certified Solutions Architect - Associate (SAA-C02) exam and passed it. Finally, I can decorate my LinkedIn profile with a cute badge, lol. In this post, I am going to talk about following subjects.

- My Background & My Study Period
- Information about the exam
- What about the exam language? (for those whose native language is not English)
- Study tips
- Study Materials
- Benefits after passing the exam

## My Background & My Study Period
**I have been working as a software engineer** in South Korea. In my company, we have our own solutions and data centers so I don't use AWS for my work. But still, I found that my experience at work helped me a lot while studying for the exam. Because most software engineers face the common issues (high availability, database, tight coupling etc), and we have common solutions for those issues (L4 loadbalancer, MySQL, Kafka etc). And **what AWS provides are also the solutions for those common issues** (Elastic Load Balancer, Amazon RDS, SQS). In that sense, my work experience helped me a lot.

In my free time, I sometimes work on my side project on AWS as my hobby. My experience with AWS is about 1 year for now. I was using the AWS services but I did not have a firm understanding of AWS system. Most of time I had to go through a lot of trial-and-error which were pretty time-consuming for my side project. So **I decided to take the exam so that I can have a firm understanding of AWS system** and preparing for the exam actually helped a lot for me (especially, I have learned a lot about IAM and VPC). For me, spending some money for exam registration acted as a booster and that made me study harder than I would if I didn't register the exam.

I planned my study plan to be 2~3 weeks. I have to work on weekdays so I used my evening time to study. I used AWS skill builder course, Youtube videos, and AWS documentations as my study material. In the day before the exam day, I went through the questions from exam dumps. When I met unfamiliar topic while studying, I tried to understand the topic by reading documentations or watching related youtube video.

I scored 804 in the exam. The pass/fail result was available immediately after the exam, which can make your day after the exam.

## Information about the exam

![Outline of AWS certifications](/assets/images/aws/aws-cert-outline.PNG){:style="display:block; margin-left:auto; margin-right:auto"}

Three are three levels of certifications: Foundational, Associate, and Professional.
Foundational, Associate, and Professional cost 100 USD, 150 USD, and 300 USD, respectively.

I skipped the Cloud Practitioner certification because I wanted to save my day. Since there was no exam available on weekend(at least in test centers where I live nearby), I had to take a day off to take the exam.

If you pass any of the exams above, they provide a **50% discount voucher** for another exam which can be used for any of the exams above. So I took the Solutions Architect Associate exam first. Because the cost would be same if I took the exam in order of Associate -> Professional (150 + 300 * 0.5), or just only Professional (300). It takes more time, but still it seems to be a safer way to get a Professional certificate.

Outline of Solutions Architect Associate exam
- Number of questions: 65 (either multiple choice or multiple response)
- Time: 130 minutes
- Cost: 150 USD
- Score: 100~1000 (should score over 720 to pass the exam)
- Topics:
  - Domain 1: Design Resilient Architectures
  - Domain 2: Design High-Performing Architectures
  - Domain 3: Design Secure Applications and Architectures
  - Domain 4: Design Cost-Optimized Architectures

## What about the exam language? (for those whose native language is not English)

My native language is Korean. AWS Certification offers test in Korean. But I took the exam in English because it is much harder for me to understand the translated terminology.

AWS Certification **offers 30 minute extension for non native speakers if you request in advance**. I didn't request more time because I had to make a phone call to a foregin number. For me, time was enough despite my English reading speed is slower than native speakers. But this information could be helpful to those who need more time.

It also helped me a lot to eliminate the wrong answers during the exam since there are a lot of text to process in each question. 

## Study tips
- Go through exam dumps (some answers were wrong in the dumps I found... if you have any doubt, it'd be better to investigate deeper)
- If you encounter an unfamiliar topic, dive deeper into that topic.
- Youtube video, AWS skill builder video are helpful.

## Study Materials
I used the following materials to study. I didn't have a firm understanding about VPC and IAM before I prepare for the exam. Fortunately, there were a lot of excellent videos explaining those topics, which helped me learn a lot. 
- [AWS VPC Beginner to Pro - Virtual Private Cloud Tutorial](https://www.youtube.com/watch?v=g2JOHLHh4rI)
- [AWS IAM Tutorial \| AWS Identity And Access Management \| AWS Tutorial For Beginners \| Simplilearn](https://www.youtube.com/watch?v=GjVFf83dcE8)
- [Architect Learning Plan](https://explore.skillbuilder.aws/learn/public/learning_plan/view/78/architect-learning-plan)
  - "Exam Readiness: AWS Certified Solutions Architect – Associate (Digital)" helped a lot.
- [Exam dumps](https://www.freecram.net/exam/SAA-C02-amazon-aws-certified-solutions-architect-associate-saa-c02-exam-e11544.html)

## Benefits after passing the exam

![After you pass the exam](/assets/images/aws/after-pass.PNG){:style="display:block; margin-left:auto; margin-right:auto"}

After you pass the exam, you can get the followings.

- Certification (valid for 3 years)
- 50% Discount voucher on the next exam
- Free practice exam voucher
- Apply to join the SME program
- Access to AWS certified community
- Can buy exclusive goods (such as t-shirts, pens, notebooks)
- Cute badge ✨ on LinkedIn profile
- Feeling of Accomplishment 🎉

![Posting a cute badge on LinkedIn profile](/assets/images/aws/aws-linkedin.PNG){:style="display:block; margin-left:auto; margin-right:auto"}

In my opinion, the most valuable thing you can get preparing the exam would be knowledeges and understanding of AWS services. Personally, I think this will help a lot for my side project from now on.

