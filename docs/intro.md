# SecurityAware Project

> SecurityAware aims to make systems and humans more aware of security problems in codebases and in source code control systems.

### $ our vision

<img src="https://raw.githubusercontent.com/cmusv/SecurityAware/f17970d85c78fba1d775dc7975116e5fa4a38f48/docs/assets/architecture.svg?token=ABTLT5MEHSC7Q3KJNDJGJYLDQEJRE">

We have been exploring several different threads under the SecurityAware project:

* [Comparison] The limitations of machine and deep learning models for security program analysis.
  * Comparison and exploration of models like Code2vec, CodeBERT, GraphCodeBERT, and more.
* [Data Characteristics] The impact of different data characteristics in the different models.
  * Class distribution, Granularity, Multiplicity, Project Diversity, Vulnerability Localization.
* [ML Framework] How to build an effective and effecient ML pipeline for security program analysis.
* Data availability and augmentation:
  * [[Security Patches Dataset](security_patches_dataset.md)] Dataset exploration (NVD, OSV, CVE Details, Mend.io, Devign, SecBench, BigVul, and more).
  * [SecFixMiner] Using Named Entity Recognition (NER) to extract information from commit messages.
  * How to improve security commit messages to foster data collection.
    * Best Practices ([SECOM](https://tqrg.github.io/secom/), [SECOMlint](https://tqrg.github.io/secomlint/))
  * Mutations
* Robustness and explainability of ML models
* How to use machine learning to improve/help static analysis approaches
