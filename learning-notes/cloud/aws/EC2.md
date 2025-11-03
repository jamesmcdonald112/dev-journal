---
title: EC2
type: learning-note
status: draft
date: 2025-11-03
updated: 2025-11-03
summary:
tech_stack:
keywords:
---
# EC2

## What I Learned
EC2 is Amazon's elestactic compute cloud. I believe they are virtual computers that allow ypoun to host your applkications and provide the compute power taht the business needs. They are highy flexible, cost-effective and quick, vs setting up your or infastructre and maintaing it. All you need to do is request the EC2 instances you want and they will be built in a few minutes.

EC2s are Vitrual machines (VMs) and they all share an underlying host machine with multipel other instances, a concept called multitenancy. In a multitenancy invoirnment , you need ot ensure that each vm is isolated from each other but is still able to share the resources provided by the host.

A hyper visor tkaes care of this resource sharing and isolation, which runs on the host machine.

For EC2, AWS manages the underlying host, the hypervosior and the isolation from instance to instance.Even though this wont be a responsibilty of anyone outside aws, it is importatn to grasp the concept.

## Example / Code Snippet
Add an example or code block here.

## Why It Matters
Explain how this connects to your projects or knowledge.

## Related 

## References
- [[#|GitHub Repository]]
- [[#|Live Demo]]
- [Official Docs](https://react.dev)
- [[#|Article]]
- [[#|Video]]
- Obsidian link