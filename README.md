# Formalizing Social Engineering Attacks in the Symbolic Model

GitHub repository for a seminar on formalizing social engineering attacks in the symbolic model at the University of Udine. This seminar is based on the article "Modeling Human Errors in Security Protocols" by David Basin, Saša Radomirović, and Lara Schmid, presented at CSF 2016.

> Reference to the original paper:
> D. Basin, S. Radomirovic and L. Schmid, "Modeling Human Errors in Security Protocols," _2016 IEEE 29th Computer Security Foundations Symposium (CSF)_, Lisbon, Portugal, 2016, pp. 325-340, doi: 10.1109/CSF.2016.30.

## Table of Contents

- [Introduction](#introduction)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Article](#article)
- [Presentation](#presentation)
- [Citation](#citation)
- [License](#license)

## Introduction

This repository provides all resources related to a seminar on formalizing social engineering attacks in the symbolic model. The seminar first introduces the Tamarin Prover for verifying security properties in the Dolev Yao model, and then explores how human errors can be modeled within the syntax of the tool.

## Repository Structure

The repository is organized as follows:

```
.
├── article
│   ├── article.pdf
│   └── src
│       ├── article.tex
│       ├── images
│       │   ├── diagrams.drawio
│       │   ├── FallibleHumans.png
│       │   ├── gui.png
│       │   ├── socialengineeringattacks.png
│       │   └── undecidability.png
│       └── references.bib
├── presentation
│   ├── slides.pdf
│   └── slides.pptx
├── README.md
├── CITATION.cff
└── LICENSE
```

- `article/`: Contains the seminar article PDF and its source files.
- `presentation/`: Contains the seminar presentation in PDF and PPTX formats.
- `README.md`: This README file.
- `CITATION.cff`: Citation information for this repository.
- `LICENSE`: The license under which this project is distributed.

## Getting Started

To explore the contents of this repository, follow the steps below:

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/dambrosidenis/Formalizing_Social_Engineering_Attacks.git
   cd Formalizing_Social_Engineering_Attacks
   ```

2. **View the Article:**

   Open the `article/article.pdf` file to read the full seminar article.

3. **View the Presentation:**

   Open the `presentation/slides.pdf` or `presentation/slides.pptx` file to review the seminar presentation.

## Article

The detailed seminar article explaining the formalization of social engineering attacks in the symbolic model can be found in the `article/` directory:

- [Article PDF](article/article.pdf)

The source files for the article, including LaTeX and images, are located in `article/src/`.

## Presentation

The presentation summarizing the seminar can be found in the `presentation/` directory:

- [Slides PDF](presentation/slides.pdf)
- [Slides PPTX](presentation/slides.pptx)

## Citation

If you use this work in your research, please cite it as follows:

```
@software{D_Ambrosi_Turing_Completeness_of_2023,
    author = {D'Ambrosi, Denis},
    month = jun,
    title = {{Formalizing Social Engineering Attacks in the Symbolic Model}},
    url = {https://github.com/dambrosidenis/Formalizing_Social_Engineering_Attacks},
    version = {1.0.0},
    year = {2023}
}
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
