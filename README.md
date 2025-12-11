# Drug Self-Administration in Head-fixed Mice

*This repository aggregates all code, hardware designs, and supporting resources for the Nature Protocols paper: "Drug self-administration in head-fixed mice" by Elizabeth M. Doncheck et al. (2025). For the full protocol, see the [published article](https://doi.org/10.1038/s41596-025-XXXXX).*

## Overview

This meta-repository serves as a centralized hub for reproducible implementation of the head-restrained intravenous and oral self-administration protocol in mice. It links to modular components of the REACHER open-source behavioral software stack, custom hardware designs (including 3D-printable parts via Tinkercad), and optimized surgical protocols for catheter implantation.

For detailed step-by-step instructions, refer to the paper's protocol sections on equipment construction, software setup, surgery, and behavioral training.

## Resources

| Repository | Description | Version | DOI |
|-----------------|-------------|---------| ----- |
| [`reacher-firmware`](https://github.com/otis-lab-musc/reacher-firmware) | Low-level drivers for interfacing with custom rigs, solenoids, pumps, and sensors (e.g., Arduino-compatible). | v1.0.1 | |
| [`reacher`](https://github.com/otis-lab-musc/reacher) | Core server for session management, event logging, and real-time control of self-administration trials. Supports Python-based extensions for custom rewards. | v1.1.1-beta | |
| [`labrynth`](https://github.com/otis-lab-musc/labrynth) | Pre-built modifiable application. | v1.0.1 | |
| [`reacher-hardware-models`](https://github.com/otis-lab-musc/`reacher-hardware-models) | 3D models for various hardware prints. | v1.0.0 | [17903383](https://doi.org/10.5281/zenodo.17903383) |

## Citation
If using these resources, please cite:

Doncheck, E.M. et al. Drug self-administration in head-fixed mice. *Nat. Protoc.* (2025). https://doi.org/10.1038/s41596-025-XXXXX

---

*Last updated: December 2025.*
