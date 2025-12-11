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
| Hardware Layer | Low-level drivers for interfacing with custom rigs, solenoids, pumps, and sensors (e.g., Arduino-compatible). | [https://github.com/otis-lab-musc/reacher-firmware](https://github.com/otis-lab-musc/reacher-firmware) | v1.0.1 |
| Backend Layer | Core server for session management, event logging, and real-time control of self-administration trials. Supports Python-based extensions for custom rewards. | [https://github.com/otis-lab-musc/reacher](https://github.com/otis-lab-musc/reacher) | v1.1.1-beta |
| Frontend Layer | Pre-built modifiable application. | [https://github.com/otis-lab-musc/labrynth](https://github.com/otis-lab-musc/labrynth) | v1.0.1 |

## Hardware Resources (Custom Equipment and Designs)

Hardware includes 3D-printable components for head-fixed rigs, catheter holders, and behavioral chambers. Designs are hosted on Tinkercad for easy editing/exporting (STL/OBJ files downloadable).

| Category       | Item                  | Description |
|----------------|-----------------------|-------------|
| Self-Ad Lever | [Head](https://www.tinkercad.com/things/g9S56UJFegB-lever-head-bigger-pin-holes-shorter-neck?sharecode=3f_-nbBbg9UC7aioIfLNltHPZ-AdJy6bjkCIR7vWI-Q) | The "C" and "NO" wires will come out through the holes on the bottom of the heads. The neck connector part of the design was also shortened. |
| Self-Ad Lever | [Lid](https://www.tinkercad.com/things/1IttoAP6Jgh-lever-head-with-pin-holes-lid-four?sharecode=ORefXBP1tT34QfFkmW87Mb6RE0QInzEFrH12TdGl5MU) | Lid part to fix down the switches in the lever head. Screw with a M3 nut and 16mm (or longer) screw. The design will print 4 lids at a time, break to use individual parts. |
| Self-Ad Lever | [Neck](https://www.tinkercad.com/things/atCbAN3qPrG-self-ad-lever-neckextended) | Extended neck part to accommodate wider range for lever positioning. Screw to the lever head with a M6 screw and nuts. |
| Self-Ad Lever | [Bridge](https://www.tinkercad.com/things/k9JXWjjSBca-lever-bridge-tallerlonger) | Height & length will accommodate wider range or lever positioning. |
| Vein Elevator | [Assembled](https://www.tinkercad.com/things/0KW05ZPI27y-vein-elevator-assembled?sharecode=Q-moSc9KoYEFl_gl1s84hQnKmEgg64IgKRjmK8CDS7g) | N/A |
| Vein Elevator | [Needle Ball](https://www.tinkercad.com/things/aUsikGU1ZDo-vein-elevator-needle-ball?sharecode=ytcvs0CzlUPBrMFLsvbjV5CQrvsRON37BZK2u_21IVs) | We cut out the needle screw part of a 3 mL syringe and placed it in the hole of the ball using super glue (photo). Use a higher infill setting to make a weighted ball for a stabler support. |
| Vein Elevator | [Needle Base](https://www.tinkercad.com/things/gi74htAHRRk-vein-elevator-needle-base?sharecode=yLKLZlWOylRxLU7Pn-HnAqAkLAdccg-yJ9DKkvshAlg) | Fill the inner space of the base with play-doh to hold the needle ball in the desired direction (photo). |
| Vein Elevator | [Spatula Base](https://www.tinkercad.com/things/6z980RmpZh1-vein-elevator-spatula-base?sharecode=3bVDkY7gXg6tAnq8_OhOVHzFHoskCaOq0JdD1aAvSyY) | Adjust the negative space in the design to accommodate the spatula width, if needed. Fill the inner space of the base with play-doh to hold the spatula in the desired direction (photo). |
| Neck Brace    | [All](https://www.tinkercad.com/things/32bZpDuUKR5-neck-brace-all?sharecode=Po2Pf2MQPpFxTbSjoDRRAUPuamFmqMjYjWO83hiraG8) | N/A |
| Neck Brace    | [Small](https://www.tinkercad.com/things/05LfrFp4PsK-neck-brace-small?sharecode=LqBNBiOfiYVT-l4O138uA2S9b_riQ7rPFQ-Vez7EKaQ) | N/A |
| Neck Brace    | [Medium](https://www.tinkercad.com/things/3nNTGS3YiZr-neckbrace-medium?sharecode=7gR8MkrxgMbK6GxFHuqEvKvfn4UAclP-cqGT1l6KvSg) | N/A |
| Neck Brace    | [Large](https://www.tinkercad.com/things/joYmOhSdOkX-neck-brace-large?sharecode=KFtGbRRkCb867ig2uwKpSZFOCOzUy69KeQP72fJHUio) | N/A |
| Neck Brace    | [X-Large](https://www.tinkercad.com/things/abpUhyhIVTi-neck-brace-x-large?sharecode=013CoxKKXfxFQhmvbQe5RridEQlwBQvItjxVXMHOPIM) | N/A |
| Syringe Pump  | [Assembled](https://www.tinkercad.com/things/hrQGc3pAiuS-syringe-pump-assembled?sharecode=HxW8IsWcEVxIW8Y0Y9D7e8OAl_QqTpfAwITRBjB7Dig) | N/A |
| Syringe Pump  | [Motor End](https://www.tinkercad.com/things/ahZJqg5fUSz-syringe-pump-motor-end?sharecode=0ZWmieIFEHn1wkXR7VuDWERRqpEmlG_VVhcFc5jzZ68) | N/A |
| Syringe Pump  | [Carriage](https://www.tinkercad.com/things/7QdFN5PZfec-syringe-pump-carriage?sharecode=K4CSwom9V3dRDHDIf0B7krZ-EkVjdi2fTTmEU2CGl_k) | N/A |
| Syringe Pump  | [Clip](https://www.tinkercad.com/things/i0xDigRkMtA-syringe-pump-clip?sharecode=hdokwclTeVyDT99rmcBfRG2Va9XiicSXbXKPUf2SQ3g) | Adjust the negative space in the design to accommodate the syringe diameter. |
| Syringe Pump  | [Body Holder-semicircle](https://www.tinkercad.com/things/56Rw1zk7RW0-syringe-pump-body-holder-semicircle?sharecode=uL9mcwIm96omMYAyDUNfd16uekKihKGoSHS57bHkbM8) | Put an insulation tape on the curvature where the syringe sits to provide a better grip. Adjust the negative space in the design to accommodate the syringe diameter. |
| Syringe Pump  | [Idler End](https://www.tinkercad.com/things/dX7hNrVUZNe-syringe-pump-idler-end?sharecode=ntST5qnH7mVibbpjF_m_as_2pC1kAojlEQKoF-HKewg) | N/A |
| Arduino Box   | [Arduino Box-body](https://www.tinkercad.com/things/29hkz7EdkGp-syringe-pump-arduino-box-body?sharecode=x0P0fa7Vhg2isAKlk38qh4uTT3rkyXujb121iI0CjE8) | Remove the plastic holder on the bottom of the Arduino (Uno3) and screw in the box using M3 screws (photo). |
| Arduino Box   | [Arduino Box-lid](https://www.tinkercad.com/things/gyZvSXLR6w5-syringe-pump-arduino-box-lid?sharecode=WLPMqxOA1e-1rc8I_iSberrh9VZR9BSsLyxCIEBL0Mk) | N/A |

## Citation
If using these resources, please cite:

Doncheck, E.M. et al. Drug self-administration in head-fixed mice. *Nat. Protoc.* (2025). https://doi.org/10.1038/s41596-025-XXXXX

For this repo: [Zenodo record](https://doi.org/10.5281/zenodo.XXXXXXX).

---

*Last updated: December 2025.*
