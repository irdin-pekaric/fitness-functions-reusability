# FAIR Artifact Checker

This repository provides a modular framework for assessing the **FAIRness** of research software artifacts based on their metadata and repository structure. The framework includes four main components:

-  `FindabilityChecker`: Tests whether artifact URIs are resolvable and reachable.
-  `AccessibilityChecker`: Verifies that artifacts can be downloaded and saved locally.
-  `InteroperabilityChecker`: Assesses whether a software artifact can be executed in a reproducible environment.
-  `ReproducibilityChecker`: Checks for the presence of essential metadata and configuration files to ensure scientific reproducibility.

The goal is to help researchers, repository maintainers, and automated tools quickly evaluate the usability and sustainability of digital artifacts.

---

## Structure

.
├── check_findability.py
├── check_accessibility.py
├── check_interoperability.py
├── check_reproducibility.py
├── artifacts.json
└── README.md


## Prerequisites

- Python 3.8+
- `requests` library
- `packaging` and `pkg_resources` (from `setuptools`)
- Internet access to reach public repositories

Install dependencies:

```bash
pip install requests packaging setuptools
```

It is also necessary to specify the repositories to be evaluated in the artifacts.json file