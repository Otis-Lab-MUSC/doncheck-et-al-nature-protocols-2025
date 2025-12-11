# Doncheck et al. (2025) - Drug Self-Administration in Head-fixed Mice

[![DOI](https://zenodo.org/badge/latestdoi/1234567.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
*This repository aggregates all code, hardware designs, and supporting resources for the Nature Protocols paper: "Drug self-administration in head-fixed mice" by Elizabeth M. Doncheck et al. (2025). For the full protocol, see the [published article](https://doi.org/10.1038/s41596-025-XXXXX).*

## Overview

This meta-repository serves as a centralized hub for reproducible implementation of the head-restrained intravenous and oral self-administration protocol in mice. It links to modular components of the REACHER open-source behavioral software stack, custom hardware designs (including 3D-printable parts via Tinkercad), and optimized surgical protocols for catheter implantation.

Key features:
- **Modular Design**: Easily adapt circuits in the REACHER stack for experimental needs.
- **Accessibility**: Equipment built from common lab supplies or 3D-printed; software is open-source and extensible.
- **Reproducibility**: All resources are versioned; cite this repo via the Zenodo DOI above for persistence.

For detailed step-by-step instructions, refer to the paper's protocol sections on equipment construction, software setup, surgery, and behavioral training.

## Software Resources (REACHER Suite)

The REACHER Suite framework is divided into hardware interfacing (backend), control logic (backend), and user interface (frontend). Below is a table of core repositories:

| Repository Name | Description | Link | Version |
|-----------------|-------------|------|---------|
| Hardware Layer | Low-level drivers for interfacing with custom rigs, solenoids, pumps, and sensors (e.g., Arduino-compatible). | [Hardware Layer Repo](https://github.com/otis-lab-musc/reacher-firmware) | v1.0.1 |
| Backend Layer | Core server for session management, event logging, and real-time control of self-administration trials. Supports Python-based extensions for custom rewards. | [Backend Layer Repo](https://github.com/otis-lab-musc/reacher) | v1.1.1-beta |
| Frontend Layer | Pre-built modifiable application. | [Frontend Layer Repo](https://github.com/otis-lab-musc/labrynth) | v1.0.1 |

## Hardware Resources (Custom Equipment and Designs)

Hardware includes 3D-printable components for head-fixed rigs, catheter holders, and behavioral chambers. Designs are hosted on Tinkercad for easy editing/exporting (STL/OBJ files downloadable).

| Category       | Item                  | Tinkercad Link                                                                 |
|----------------|-----------------------|--------------------------------------------------------------------------------|
| Self-Ad Lever | Head                 | [https://www.tinkercad.com/things/g9S56UJFegB-lever-head-bigger-pin-holes-shorter-neck?sharecode=3f_-nbBbg9UC7aioIfLNltHPZ-AdJy6bjkCIR7vWI-Q](https://www.tinkercad.com/things/g9S56UJFegB-lever-head-bigger-pin-holes-shorter-neck?sharecode=3f_-nbBbg9UC7aioIfLNltHPZ-AdJy6bjkCIR7vWI-Q) |
| Self-Ad Lever | Lid                  | [https://www.tinkercad.com/things/1IttoAP6Jgh-lever-head-with-pin-holes-lid-four?sharecode=ORefXBP1tT34QfFkmW87Mb6RE0QInzEFrH12TdGl5MU](https://www.tinkercad.com/things/1IttoAP6Jgh-lever-head-with-pin-holes-lid-four?sharecode=ORefXBP1tT34QfFkmW87Mb6RE0QInzEFrH12TdGl5MU) |
| Self-Ad Lever | Neck                 | [https://www.tinkercad.com/things/atCbAN3qPrG-self-ad-lever-neckextended](https://www.tinkercad.com/things/atCbAN3qPrG-self-ad-lever-neckextended) |
| Self-Ad Lever | Bridge               | [https://www.tinkercad.com/things/k9JXWjjSBca-lever-bridge-tallerlonger](https://www.tinkercad.com/things/k9JXWjjSBca-lever-bridge-tallerlonger) |
| Vein Elevator | Assembled            | [https://www.tinkercad.com/things/0KW05ZPI27y-vein-elevator-assembled?sharecode=Q-moSc9KoYEFl_gl1s84hQnKmEgg64IgKRjmK8CDS7g](https://www.tinkercad.com/things/0KW05ZPI27y-vein-elevator-assembled?sharecode=Q-moSc9KoYEFl_gl1s84hQnKmEgg64IgKRjmK8CDS7g) |
| Vein Elevator | Needle Ball          | [https://www.tinkercad.com/things/aUsikGU1ZDo-vein-elevator-needle-ball?sharecode=ytcvs0CzlUPBrMFLsvbjV5CQrvsRON37BZK2u_21IVs](https://www.tinkercad.com/things/aUsikGU1ZDo-vein-elevator-needle-ball?sharecode=ytcvs0CzlUPBrMFLsvbjV5CQrvsRON37BZK2u_21IVs) |
| Vein Elevator | Needle Base          | [https://www.tinkercad.com/things/gi74htAHRRk-vein-elevator-needle-base?sharecode=yLKLZlWOylRxLU7Pn-HnAqAkLAdccg-yJ9DKkvshAlg](https://www.tinkercad.com/things/gi74htAHRRk-vein-elevator-needle-base?sharecode=yLKLZlWOylRxLU7Pn-HnAqAkLAdccg-yJ9DKkvshAlg) |
| Vein Elevator | Spatula Base         | [https://www.tinkercad.com/things/6z980RmpZh1-vein-elevator-spatula-base?sharecode=3bVDkY7gXg6tAnq8_OhOVHzFHoskCaOq0JdD1aAvSyY](https://www.tinkercad.com/things/6z980RmpZh1-vein-elevator-spatula-base?sharecode=3bVDkY7gXg6tAnq8_OhOVHzFHoskCaOq0JdD1aAvSyY) |
| Neck Brace    | All                  | [https://www.tinkercad.com/things/32bZpDuUKR5-neck-brace-all?sharecode=Po2Pf2MQPpFxTbSjoDRRAUPuamFmqMjYjWO83hiraG8](https://www.tinkercad.com/things/32bZpDuUKR5-neck-brace-all?sharecode=Po2Pf2MQPpFxTbSjoDRRAUPuamFmqMjYjWO83hiraG8) |
| Neck Brace    | Small                | [https://www.tinkercad.com/things/05LfrFp4PsK-neck-brace-small?sharecode=LqBNBiOfiYVT-l4O138uA2S9b_riQ7rPFQ-Vez7EKaQ](https://www.tinkercad.com/things/05LfrFp4PsK-neck-brace-small?sharecode=LqBNBiOfiYVT-l4O138uA2S9b_riQ7rPFQ-Vez7EKaQ) |
| Neck Brace    | Medium               | [https://www.tinkercad.com/things/3nNTGS3YiZr-neckbrace-medium?sharecode=7gR8MkrxgMbK6GxFHuqEvKvfn4UAclP-cqGT1l6KvSg](https://www.tinkercad.com/things/3nNTGS3YiZr-neckbrace-medium?sharecode=7gR8MkrxgMbK6GxFHuqEvKvfn4UAclP-cqGT1l6KvSg) |
| Neck Brace    | Large                | [https://www.tinkercad.com/things/joYmOhSdOkX-neck-brace-large?sharecode=KFtGbRRkCb867ig2uwKpSZFOCOzUy69KeQP72fJHUio](https://www.tinkercad.com/things/joYmOhSdOkX-neck-brace-large?sharecode=KFtGbRRkCb867ig2uwKpSZFOCOzUy69KeQP72fJHUio) |
| Neck Brace    | X-Large              | [https://www.tinkercad.com/things/abpUhyhIVTi-neck-brace-x-large?sharecode=013CoxKKXfxFQhmvbQe5RridEQlwBQvItjxVXMHOPIM](https://www.tinkercad.com/things/abpUhyhIVTi-neck-brace-x-large?sharecode=013CoxKKXfxFQhmvbQe5RridEQlwBQvItjxVXMHOPIM) |
| Syringe Pump  | Assembled            | [https://www.tinkercad.com/things/hrQGc3pAiuS-syringe-pump-assembled?sharecode=HxW8IsWcEVxIW8Y0Y9D7e8OAl_QqTpfAwITRBjB7Dig](https://www.tinkercad.com/things/hrQGc3pAiuS-syringe-pump-assembled?sharecode=HxW8IsWcEVxIW8Y0Y9D7e8OAl_QqTpfAwITRBjB7Dig) |
| Syringe Pump  | Motor End            | [https://www.tinkercad.com/things/ahZJqg5fUSz-syringe-pump-motor-end?sharecode=0ZWmieIFEHn1wkXR7VuDWERRqpEmlG_VVhcFc5jzZ68](https://www.tinkercad.com/things/ahZJqg5fUSz-syringe-pump-motor-end?sharecode=0ZWmieIFEHn1wkXR7VuDWERRqpEmlG_VVhcFc5jzZ68) |
| Syringe Pump  | Carriage             | [https://www.tinkercad.com/things/7QdFN5PZfec-syringe-pump-carriage?sharecode=K4CSwom9V3dRDHDIf0B7krZ-EkVjdi2fTTmEU2CGl_k](https://www.tinkercad.com/things/7QdFN5PZfec-syringe-pump-carriage?sharecode=K4CSwom9V3dRDHDIf0B7krZ-EkVjdi2fTTmEU2CGl_k) |
| Syringe Pump  | Clip                 | [https://www.tinkercad.com/things/i0xDigRkMtA-syringe-pump-clip?sharecode=hdokwclTeVyDT99rmcBfRG2Va9XiicSXbXKPUf2SQ3g](https://www.tinkercad.com/things/i0xDigRkMtA-syringe-pump-clip?sharecode=hdokwclTeVyDT99rmcBfRG2Va9XiicSXbXKPUf2SQ3g) |
| Syringe Pump  | Body Holder-semicircle | [https://www.tinkercad.com/things/56Rw1zk7RW0-syringe-pump-body-holder-semicircle?sharecode=uL9mcwIm96omMYAyDUNfd16uekKihKGoSHS57bHkbM8](https://www.tinkercad.com/things/56Rw1zk7RW0-syringe-pump-body-holder-semicircle?sharecode=uL9mcwIm96omMYAyDUNfd16uekKihKGoSHS57bHkbM8) |
| Syringe Pump  | Idler End            | [https://www.tinkercad.com/things/dX7hNrVUZNe-syringe-pump-idler-end?sharecode=ntST5qnH7mVibbpjF_m_as_2pC1kAojlEQKoF-HKewg](https://www.tinkercad.com/things/dX7hNrVUZNe-syringe-pump-idler-end?sharecode=ntST5qnH7mVibbpjF_m_as_2pC1kAojlEQKoF-HKewg) |
| Arduino Box   | Arduino Box-body     | [https://www.tinkercad.com/things/29hkz7EdkGp-syringe-pump-arduino-box-body?sharecode=x0P0fa7Vhg2isAKlk38qh4uTT3rkyXujb121iI0CjE8](https://www.tinkercad.com/things/29hkz7EdkGp-syringe-pump-arduino-box-body?sharecode=x0P0fa7Vhg2isAKlk38qh4uTT3rkyXujb121iI0CjE8) |
| Arduino Box   | Arduino Box-lid      | [https://www.tinkercad.com/things/gyZvSXLR6w5-syringe-pump-arduino-box-lid?sharecode=WLPMqxOA1e-1rc8I_iSberrh9VZR9BSsLyxCIEBL0Mk](https://www.tinkercad.com/things/gyZvSXLR6w5-syringe-pump-arduino-box-lid?sharecode=WLPMqxOA1e-1rc8I_iSberrh9VZR9BSsLyxCIEBL0Mk) |

## Citation
If using these resources, please cite:

Doncheck, E.M. et al. Drug self-administration in head-fixed mice. *Nat. Protoc.* (2025). https://doi.org/10.1038/s41596-025-XXXXX

For this repo: [Zenodo record](https://doi.org/10.5281/zenodo.XXXXXXX).

---

*Last updated: December 2025.*
