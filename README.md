# FAIR Artifact Checker

This repository provides a modular framework for assessing the **FAIRness** of research software artifacts based on their metadata and repository structure. Each **FAIRness** fitness function is implemented as an independent module, but all share a common design pattern:

1. **Environment setup** – establishing the conditions for executing the checks (e.g., creating a temporary directory, setting up a containerized runtime, or using standard Python libraries).  
2. **Verification steps** – defining the concrete operations used to test the FAIR criterion, and how success/failure is lo

The goal is to help researchers, repository maintainers, and automated tools quickly evaluate the usability and sustainability of digital artifacts.

---
## AccessibilityChecker

The *AccessibilityChecker* confirms that artifacts are not just identifiable but also retrievable in practice.

- **Environment setup**  
  Creates a sandboxed working directory to store downloaded files, ensuring tests do not interfere with the host system. For protected resources, authentication tokens can be configured.  

- **Verification steps**  
  1. Attempts to download the artifact from its declared source (e.g., GitHub release archive, Zenodo deposit).  
  2. Confirms the file can be saved locally without corruption (using file size checks and optional hash validation if checksums are available).  
  3. Logs specific reasons for failure (authentication required, expired link, missing permissions, or network issues).  

By combining successful retrieval with local validation, this checker verifies that research software is not only “available” but *practically accessible* under FAIR standards.  

---

## InteroperabilityChecker

The *InteroperabilityChecker* ensures that research software can be executed within a reproducible computational environment.

- **Environment setup**  
  A controlled runtime is established using containerization or virtual environments.  
  - If a `Dockerfile` is present → build a Docker image.  
  - If `environment.yml` or `requirements.txt` is detected → set up a Conda or Python virtual environment.  
  This prevents dependency installation from polluting the host system.  

- **Verification steps**  
  1. Parses declared dependencies from configuration files.  
  2. Installs all dependencies into the controlled environment.  
  3. Runs minimal execution tests (e.g., import the main Python package, invoke CLI entry points, or execute a documented example).  
  4. Logs incompatibilities, version conflicts, or missing dependencies.  

This ensures that the artifact can interoperate with common research infrastructures and be integrated into pipelines without manual troubleshooting.  

---

## ReproducibilityChecker

The *ReproducibilityChecker* evaluates whether an artifact contains sufficient metadata and configuration files to enable scientific reproducibility.

- **Environment setup**  
  Operates directly on the repository structure. No external services are required beyond a cloned copy of the repository.  

- **Verification steps**  
  1. Scans for essential documentation and metadata files (`README`, `LICENSE`, `CITATION.cff`, `CODEMETA.json`).  
  2. Checks for build or workflow definitions (`Makefile`, `Snakefile`, GitHub Actions, etc.).  
  3. Verifies the presence of example datasets, test cases, or parameter files that allow replication of published results.  
  4. Ensures metadata fields are meaningfully filled (not placeholders).  

This systematic check confirms that the artifact is not just executable but also scientifically reusable by others.  

---

## Structure

<pre>
.
├── check_findability.py
├── check_accessibility.py
├── check_interoperability.py
├── check_reproducibility.py
├── artifacts.json
└── README.md
</pre>


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
