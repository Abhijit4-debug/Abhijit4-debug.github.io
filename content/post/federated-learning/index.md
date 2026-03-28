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

I have been meaning to write this for a while. Probably should have written it two years ago when I was still in the thick of it, sleep-deprived and annoyed enough to be really precise about what was bothering me. I have lost some of that edge since then. But the opinion has not changed, and I keep seeing the same papers making the same claims, so here it is.

Most federated learning strategies that claim to improve on FedAvg do not actually improve on FedAvg. Not on real hardware. Not under real conditions. And I think a lot of people in this space quietly know that but nobody wants to be the one to say it out loud because the entire publishing pipeline runs on showing a curve that is slightly above the baseline.

I have implemented five different FL strategies, baseline and state-of-the-art, and run them on real clusters. Raspberry Pis with 1 gig of RAM. Nvidia Jetsons with GPUs. Mixed heterogeneous setups with seven different device types. IID data and non-IID data. Hours-long training sessions on 80+ physical edge devices, plus containerized runs at 200+ and 1000+ scale. And across roughly 14 combinations of models, clusters, and data distributions, FedAvg or its async variant was the best or comparable to the best in nearly all of them. The strategies with the clever tiering and the data-aware clustering and the async tier blending? About the same. Sometimes worse.

I will say that again because I think it matters: random client selection with simple averaging matches or beats the strategies specifically designed to handle the problems that random client selection supposedly cannot handle.

The reason is not that these strategies are badly conceived. The ideas are often sound. It is that they make assumptions which hold in simulation and come apart on hardware.

Tiered approaches cluster clients by training speed so you avoid stragglers. Makes sense. But the clustering assumes stable per-device latency, and on our Jetsons the same device type would swing from 260 to 310 seconds per round for the same model depending on thermals. The tiers are built on numbers that are already wrong by the time you use them.

Data-aware clustering assigns weights based on each client's data histogram and cluster loss. Also reasonable. But it assumes a shape of label distribution that may not match your actual non-IID split. We tried two different partitionings. Neither one gave it an edge over random selection.

The async tiered strategy was the most sophisticated thing we ran. Synchronous within tiers, asynchronous across them. On paper, genuinely elegant. On hardware, it kept selecting from the same performance tier, which meant the same small group of devices got hammered every round. They overheated. They froze. We had one configuration on the Jetson cluster that could not finish a multi-hour run. Tried it repeatedly. DNF. Meanwhile FedAvg, which just randomly spreads the load, finished every single time without drama.

That is the part I keep coming back to. The sophisticated strategies are not just failing to beat the baseline. Some of them introduce failure modes that the baseline does not have. Random selection is accidentally a form of load balancing. It is not optimal in any theoretical sense but it is robust, and when your training runs for hours on devices that thermally throttle and crash, robustness is the game.

I am not saying the problems are fake. Client heterogeneity is real. Non-IID convergence issues are real. Stragglers are real. The community is honest about these being hard problems. What I am saying is that the strategies published to solve them consistently fail to deliver on real hardware, and the gap between simulation results and operational results is large enough that it should change how we evaluate this work.

The other thing you learn fast when you run FL for real is that the boring stuff is what keeps you alive. Checkpointing is the difference between recovering from a server crash in under a second and losing four hours of accumulated training state. Heartbeat-based failure detection is what stops you from dispatching work to a dead node. State externalization is what lets you bring up a replacement server and resume mid-session instead of starting clean. None of this is publishable. All of it is load-bearing.

Most frameworks treat these things as a footnote. The resilience story is a flag you can enable, not a design principle. And when the server is assumed reliable and client failures are modeled as a probability distribution instead of an SD card that corrupted because a Pi lost power during a write, you end up with systems that work beautifully in evaluation sections and fall over in practice.

I think the FL strategy space is less differentiated than the literature suggests. When you remove the simulation-friendly assumptions and test on real heterogeneous hardware with real failures over real wall-clock time, the gap between FedAvg and the state of the art compresses to noise. That is not a comfortable thing to say if your research agenda depends on that gap existing. But it is what the numbers show, and I think the next round of FL systems work should start from that observation instead of pretending it is not there.
