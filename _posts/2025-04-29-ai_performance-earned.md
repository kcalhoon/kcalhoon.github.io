---
title: "How to achieve strong AI performance — diligence"
date: 2025-04-28 10:15:50 +0000
categories: 
  - production-AI
tags:
  - Rechat
  - ClassPass
  - evals
  - Rechat.com
  - ClassPass
  - performance
  - Ritchie Bros.
  - Hamel Husain
  - Emil Sedgh
---

At Ritchie Bros, a $ 4 B+ equipment auctioneer, we built an ML system "Algo" to automatically value assets from a few dollars to $ 100 K+ (think Zillow for excavators). Algo outperformed both human appraisers and the leading market valuation model, and, of course, we could revalue thousands of assets daily.

<!--more-->

This system's success took years. From modest beginnings, we improved by: 1) partnering with experts to identify valuation drivers, 2) continuously refining our models while tracking performance metrics, and 3) integrating it into the backend systems. The result? Double-digit accuracy gains quarterly, expanded asset coverage, and reduced latency.

Building performant AI systems requires this same disciplined approach. For Algo, we had an expanding auction dataset. For AI, evaluations are the flywheel that drives improvement. Some examples:

* At Rechat, Hamel Husain and Emil Sedgh slashed LLM error rates from 40 %+ to under 5% in six months through rigorous evaluation, creating benchmarks for pre-production testing and data for model fine-tuning.

* Jon Victor writes about similar success at ClassPass. The company's AI customer support chatbots in January 2024 performed worse than humans, but now match human satisfaction scores while handling 50% of the volume. They monitor 10% of AI interactions to identify improvement opportunities.

![Rechat.com LLM error rate reduction](/assets/images/rechat_error_rate_reduction_evals.png){:height="700px" width="400px"}

GenAI impresses out of the box, but performance excellence comes from deliberate improvement. And often the key metric isn't perfection but performance versus alternatives, typically human intervention.

[Rechat video](https://youtu.be/eLXF0VojuSs?si=kJIPL2vDDWkFa5Jf)
[ClassPass](https://www.theinformation.com/articles/classpass-got-support-bot-par-humans)
