# Heureka Notes for Attie Lab

- [Heureka Labs](https://heurekalabs.co/)
- [Heureka Labs Download](https://heurekalabs.co/download.html)

## People

- [Matt Hirschey](https://dmpi.duke.edu/matthew-hirschey-phd)
- [Ioan Bolohan](https://heurekalabs.co/about.html)
- [Pol Castellano Escuder](https://heurekalabs.co/about.html)
- [John Denu](https://denulab.discovery.wisc.edu/staff/denu-john/)

## Project `tryout`

- [resources.md](resources.md) - Resources and capabilities of Heureka.
- [files.md](files.md) - Other files in the `tryout` project.
- [tokens.md](tokens.md) - Token usage tracking.
- [RATIONALE.md](RATIONALE.md) - Project goals.
- [MEMORY.md](MEMORY.md) - Persistent research memory.
- [plans/](plans/) - Pre-registered, locked methodology files.
- [todos/](todos/) - Task list.
- [experiments.jsonl](experiments.jsonl) - Running log of analyses performed.

## Overview

Heureka Bench uses
Archimedes,
which is a proprietary multi-omics biology model
trained on measured biological samples rather than published text.

LLM-based reasoning models have enabled the development of agentic systems that act as co-scientists, assisting in multi-step scientific analysis. However, evaluating these systems is challenging, as it requires realistic, end-to-end research scenarios that integrate data analysis, interpretation, and the generation of new insights from the experimental data. To address this limitation, we introduce HeurekaBench, a framework to create benchmarks with exploratory, open-ended research questions for experimental datasets. Each such question is grounded in a scientific study and its corresponding code repository, and is created using a semi-automated pipeline that leverages multiple LLMs to extract insights and generate candidate workflows, which are then verified against reported findings. We instantiate the framework in single-cell biology to obtain sc-HeurekaBench benchmark and use it to compare state-of-the-art single-cell agents. We further showcase the benefits of our benchmark for quantitatively analyzing current design choices in agentic systems. We find that the addition of a critic module can improve ill-formed responses for open-source LLM-based agents by up to 22% and close the gap with their closed-source counterparts. Overall, HeurekaBench sets a path toward rigorous, end-to-end evaluation of scientific agents, grounding benchmark construction in real scientific workflows.

### Core Components in Heureka Bench

- Archimedes: “A proprietary model trained on hundreds of thousands of real biological samples — not text — so Heureka can place, QC, and contextualize your data in ways a general LLM can't.”, as noted by Heureka Labs.
- ARC (AI Research Companion): The embedded scientific agent that queries the underlying biological model to perform data analysis, statistics, and handle local research files. [1]

How does Archimedes compare to general language models like GPT-4 or Claude for biological data analysis?

## References

- [[2601.01678] HeurekaBench: A Benchmarking Framework for AI Co-scientist](<https://arxiv.org/abs/2601.01678>)
- [How to discover: A Heureka Labs experiment by Matthew Hirschey](https://handbook.heurekalabs.co/)
  - [About this Handbook](https://handbook.heurekalabs.co/about.html)
- [mlbio-epfl/HeurekaBench: [ICLR 2026] A framework to "create benchmarks" and "evaluate AI co-scientists" in experimental data-driven real-world scientific research. · GitHub](https://github.com/mlbio-epfl/HeurekaBench)
- [Skills for Scientific Discovery](https://heurekaskills.com/)
- [Inside scientific benchmarks - Heureka Labs](https://www.heurekalabs.co/blog/inside-scientific-benchmarks/)
- [lab-bench — Heureka Skills (harness info?)](https://heurekaskills.com/lab-bench/)
- [Heureka Labs YouTube Channel](https://www.youtube.com/@Heureka-Labs)
  - [Heureka! Stories of Discovery with Matthew Hirschey](https://www.youtube.com/watch?v=y82wZem60Ks)`
