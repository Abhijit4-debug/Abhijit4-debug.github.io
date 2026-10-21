---
title: ''
date: 2026-04-27
lastmod: 2026-10-20
type: landing

sections:
  - block: about.biography
    id: about
    content:
      title: Hi, I'm Abhijit!
      username: admin

  - block: resume-skills
    id: skills
    title: Skills
    content:
      username: admin
    design:
      show_skill_percentage: false

  - block: resume-exp
    title: Education
    id: education
    content:
      items:
        - title: Master of Science in Computer Science
          icon_choice: university
          id: umass
          institution: University of Massachusetts Amherst
          date_start: 2025-09-01
          date_end: 2027-05-31
          description: |
            Expected graduation: **May 2027**. Focused on **ML systems, distributed training, and scientific AI**.

            **Relevant Courses:**
            - *Robotics*
            - *Advanced Generative AI*
            - *AI Alignment*
            - *Advanced NLP*
            - *Advanced ML*
            - *Distributed Systems*

        - title: Bachelor of Technology in Computer Science
          icon_choice: university
          id: vit
          institution: Vellore Institute of Technology (VIT)
          date_start: 2020-09-01
          date_end: 2024-09-30
          description: |
            Undergraduate degree in Computer Science with focus on machine learning, distributed systems, and software engineering.

  - block: resume-exp
    title: Experience
    id: experience
    content:
      items:
        - title: Graduate Research Assistant
          icon_choice: briefcase
          id: argonne
          company: Argonne National Laboratory
          date_start: 2026-02-01
          date_end: ""
          location: Remote
          description: |
            - Develop Llama-3.1-70B and GPT-OSS-20B post-training workflows (SFT/DPO/KTO) using PyTorch FSDP, with scaling experiments up to 256 Aurora nodes (1,536 Intel GPUs); profile bottlenecks and debug out-of-memory and checkpointing failures.
            - Engineered a Kubernetes-native provisioning system on NERSC Spin for [APPFL](https://github.com/APPFL/APPFL), a DOE federated learning platform, dynamically spinning up org-scoped, oauth2-proxy-authenticated Jupyter environments with Keycloak SSO and an invite-based FastAPI/PostgreSQL onboarding pipeline.
            - Built and open-sourced [HiveWatch](https://github.com/APPFL/hivewatch/), a framework-agnostic observability toolkit for distributed ML training with real-time metric dashboards, geographic client visualization, and experiment tracking via WandB and MLflow, adopted across APPFL.
            - Engineered federated LoRA fine-tuning on the Evo-2 model pipeline for genomic AI training across institutions, reducing cross-site data transfer to 20 MB (down 74%) on Polaris/NERSC HPC.

        - title: Independent Research Collaborator
          icon_choice: briefcase
          id: fedproj
          company: "Mentors: Dr. Mahdi Morafah (UCSD), Dr. Vishnu Pandi (Purdue)"
          date_start: 2024-12-01
          date_end: 2025-09-30
          location: Remote
          description: |
            - Designed and implemented [FedProj](https://github.com/Abhijit4-debug/FedProj_TMLR), a gradient-projection algorithm for federated learning that prevents catastrophic forgetting on non-IID data, achieving 2–9% accuracy gains over state-of-the-art FL baselines across 100+ clients.
            - Orchestrated 70+ large-scale training runs across CV (CIFAR-10/100, CINIC-10) and NLP (MNLI, SST-2, MARC) benchmarks; work published in [TMLR](https://openreview.net/forum?id=lhTWPh3Tjm).

        - title: Associate Software Engineer - Security Research Team
          icon_choice: briefcase
          id: iudx
          company: Indian Urban Data Exchange, IISc Campus (IUDX-IISc)
          date_start: 2024-10-01
          date_end: 2025-07-31
          location: Bengaluru, India
          description: |
            - Architected confidential computing solutions using Multi-Party Computation, Differential Privacy, and AMD SEV-SNP TEEs with cryptographic remote attestation for healthcare federated learning deployments.
            - Deployed Confidential Containers (Peer Pods) on Kubernetes with runtime attestation, enabling privacy-preserving ML across multi-cloud infrastructure while reducing setup time by 60%.

        - title: Federated Learning Research Intern
          icon_choice: briefcase
          id: dreamlab
          company: Indian Institute of Science, DREAM:LAB
          date_start: 2023-12-01
          date_end: 2024-09-30
          location: Bengaluru, India
          description: |
            - Co-developed [Flotilla](https://github.com/dream-lab/flotilla/), a modular federated learning framework for heterogeneous resources.
            - Executed 90+ experiments on heterogeneous clusters (46 Raspberry Pis, 12 Nvidia Jetsons) benchmarking IID and non-IID performance.
            - Scaled Flotilla to 1,024 AWS clients, achieving 55% lower training-round latency than Flower in benchmarks by removing serialization in the communication batching path.
            - Designed CPU and memory visualizations contributing to a publication in Elsevier JPDC.

        - title: NLP Research Intern
          icon_choice: briefcase
          id: carelon
          company: Carelon Global Solutions (Elevance Health)
          date_start: 2023-05-01
          date_end: 2023-09-30
          location: Hyderabad, India
          description: |
            - Engineered a Memory Module into ClinicalBERT, DistilBERT, RoBERTa, and Clinical-Longformer for sentiment analysis, achieving 90% accuracy.
            - Transitioned centralized models to federated learning, improving accuracy by 2% and reducing training time by 30% with LoRA, P-Tuning, and Prompt Tuning.

  - block: markdown
    id: awards
    content:
      title: Awards
      text: |
        **Best Poster Award · IEEE eScience 2026**

        *[Towards Global Federated Genome-Wide Association Meta-Analysis Using GA4GH TES](https://arxiv.org/pdf/2609.02227)*

        Abhijit Chunduru, Matthew Joel, Zilinghan Li, and Ravi Madduri.
    design:
      columns: '2'

  - block: collection
    id: projects
    content:
      title: Selected Projects
      filters:
        folders:
          - project
    design:
      columns: '2'
      view: card

  - block: collection
    id: publications
    content:
      title: Publications
      filters:
        folders:
          - publication
    design:
      columns: '2'
      view: card
---
