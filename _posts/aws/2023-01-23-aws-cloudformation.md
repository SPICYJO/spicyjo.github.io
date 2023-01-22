---
layout: post
title: AWS CloudFormation
description: What is aws CloudFormation?
author: Seungwoo Jo
# last_modified_at: 2022-01-01 00:00:00 +0900
# math: false
tags: aws, cloudformation
category: aws
comments: true
---

This is what I learned about AWS CloudFormation today through AWS Skill Builder learning material.

## What does CloudFormation do?
- Infrastructure as code
  - Model a collection of related AWS and third-party resources
  - Provision them quickly and consistently
  - Manage them throughout their lifecycle
- Define AWS resources in either YAML or JSON, called a CloudFormation template
- Then you canm create a CloudFormation stack in AWS
- CloudFormation canalso create Change Sets for approval before making the changes

## Benefits of CloudFormation
- Automate best practices
- Scale infrastructure worldwide
- Integrate with other AWS services
- Manage third-party and private resources
- Extend CloudFormation with the community

## How can I use CloudFormation?
- Single Devs - Avoid costs
  - CloudFormation can help quickly create and destory a group of related resources
- Enterprise - Infrastructure as code
- Disaster Recovery

## Three things to be aware of
- How to create the CloudFormation stacks
  - Manually
  - Integration pipeline (better)
- How the lifecycle of each resource is managed
- Manually updating resources that belong to a CloudFormation stack is strongly discouraged

## Cost of CloudFormation
- CloudFormation is free for managing AWS resources
- Only charged for the resources you create and the API calls that CloudFormation performs on your behalf

# Using CloudFormation

## Basic concepts
- Resources
- Templates
- Stack
- StackSet