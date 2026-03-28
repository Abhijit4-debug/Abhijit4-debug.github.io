---
title: "Federated Learning Looks Different at 3am When Your Nodes Are Dropping"
summary: 'Most FL strategies that claim to improve on FedAvg do not actually improve on FedAvg. Not on real hardware. Not under real conditions.'
date: 2026-03-27
math: false
toc: false
tags:
- federated learning
- distributed systems
- systems research
- edge computing
---

I have been meaning to write this for a while, probably should have done it two years ago when I was still in the thick of it, sleep-deprived and annoyed enough to be really precise about what was bothering me. I have lost some of that edge since then, but the opinion has not changed, and I keep seeing the same papers making the same claims, so here it is.

Most federated learning strategies that claim to improve on FedAvg do not actually improve on FedAvg when you run them on real hardware under real conditions. I think a lot of people in this space quietly know that, but nobody wants to be the one to say it out loud because the entire publishing pipeline runs on showing a curve that sits slightly above the baseline.

I have implemented five different FL strategies, both baseline and state-of-the-art, and run them on real clusters with Raspberry Pis that have 1 gig of RAM sitting next to Nvidia Jetsons with GPUs, across mixed heterogeneous setups spanning seven different device types. IID data, non-IID data, Dirichlet-based skew, hours-long training sessions on 50+ physical edge devices, plus containerized runs at 200+ and 1000+ scale. And across roughly 14 combinations of models, clusters, and data distributions, FedAvg or its async variant was the best or comparable to the best in nearly all of them, while the strategies with the clever tiering and the data-aware clustering and the async tier blending came in at about the same level or sometimes worse.

I will say that again because I think it matters: random client selection with simple averaging matches or beats the strategies specifically designed to handle the problems that random client selection supposedly cannot handle.

The reason is not that these strategies are badly conceived, because the ideas are often quite sound, but rather that they make assumptions which hold in simulation and come apart the moment real hardware is involved.

Tiered approaches cluster clients by training speed so you avoid stragglers, which makes sense on paper, but the clustering assumes stable per-device latency, and on our Jetsons the same device type would swing from 260 to 310 seconds per round for the same model depending on thermals and background load. The tiers are built on numbers that are already wrong by the time you use them, and the selection logic ends up working off a stale picture of the cluster.

Data-aware clustering assigns weights based on each client's data histogram and the cluster's average training loss, which is also reasonable until you realize it assumes a shape of label distribution that may not match your actual non-IID split. We tried two different partitionings and neither one gave it an edge over just selecting randomly.

The async tiered strategy was the most sophisticated thing we ran, doing synchronous training within performance tiers and asynchronous aggregation across them, which is genuinely elegant as a design. On hardware though, it kept selecting from the same performance tier, which meant the same small group of devices got hammered every round until they overheated and froze. We had one configuration on the Jetson cluster that could not finish a multi-hour run no matter how many times we tried it, while FedAvg, which just randomly spreads the load across the pool, finished every single time without drama.

That is the part I keep coming back to: the sophisticated strategies are not just failing to beat the baseline, some of them are introducing failure modes that the baseline does not have. Random selection turns out to be an accidental form of load balancing, and while it is not optimal in any theoretical sense, it is robust, which matters a lot more when your training runs for hours on devices that thermally throttle and crash.

I am not saying the problems these strategies target are fake, because client heterogeneity is real and non-IID convergence issues are real and stragglers absolutely slow you down, and the community is honest about all of these being hard problems. What I am saying is that the strategies published to address them consistently fail to deliver on real hardware, and the gap between simulation results and operational results is large enough that it should change how we evaluate this work.

The other thing you learn fast when you run FL for real is that the boring infrastructure is what keeps you alive. Checkpointing is the difference between recovering from a server crash in under a second and losing four hours of accumulated training state, heartbeat-based failure detection is what stops you from dispatching work to a dead node, and state externalization is what lets you bring up a replacement server and resume mid-session instead of starting clean. None of this is publishable work and all of it is completely load-bearing, but most frameworks treat it as a footnote where the resilience story is a flag you can enable rather than a design principle baked into the system.

When the server is assumed reliable and client failures are modeled as a probability distribution instead of an SD card that corrupted because a Pi lost power during a write, you end up with systems that work beautifully in evaluation sections and fall over in practice.

I think the FL strategy space is less differentiated than the literature suggests, and when you remove the simulation-friendly assumptions and test on real heterogeneous hardware with real failures over real wall-clock time, the gap between FedAvg and the state of the art compresses to noise. That is not a comfortable thing to say if your research agenda depends on that gap existing, but it is what the numbers showed, and I think the next round of FL systems work should start from that observation instead of pretending it is not there.
