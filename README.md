# **A Research Exploration: From Physical Modeling to AI for Beam Damage Identification**
#### **Author**: Jiaye Liang (Undergraduate)
#### **Project Status**: An open-ended research project.
#### **Primary Goal**: To document my learning process and invite discussion on a challenging inverse problem.
#### **Email**: 2208341591@qq.com

---

## **Why is this project open-sourced?**
This repository captures my first major attempt to bridge structural mechanics and data-driven methods. The outcomes are not final answers but documented steps, questions, and findings. I am releasing it to solicit feedback, invite constructive criticism, and hope to connect with researchers who find the underlying problems interesting. All code and thoughts are provided “as is” for transparent academic discussion.

---

## **Project Vision: An Exploratory Journey**
This project documents my attempt to tackle a classic yet difficult problem in Structural Health Monitoring (SHM): “**Can we identify localized damage in a beam using only its low-order vibration modes ?**” \
Instead of seeking a final solution, I aimed to build a **transparent**, **physics-grounded pipeline** to investigate this question. The work follows a research-like progression: defining the problem, building a verified physical model, introducing realistic complexities, and finally exploring where data-driven methods succeed and—more importantly—where they currently struggle.

---

## **What I Built?**
To conduct this investigation, I built a controllable environment. I already developed the following components:

- **A Reliable Physical Simulator** (code name: /1D_Timoshenko_beam_FEM/)
  - Implemented a 1D Timoshenko beam Finite Element Method (FEM) solver from scratch in Python.
  - Key Validation: Cross-checked results with ANSYS APDL (code name: /1D_Timoshenko_beam_APDL/), achieving a frequency error of <0.15%, ensuring subsequent observations are physically meaningful.
- **A Model for Real-World Material Uncertainty** (code name: /1D_Timoshenko_beam_KLexpansion/)
  - Integrated Karhunen-Loève (KL) Expansion to generate spatially correlated, random stiffness fields (log-normal distribution). This moves beyond idealized “uniform” material properties.
- **A Library of Parametric “Damages”** (code name: /1D_Timoshenko_beam_mutation/)
  - Created a ```class MutationLibrary``` to simulate various engineering issues (e.g., loose joints, cracks, corrosion) as localized stiffness reductions. This allows for systematic generation of different damage scenarios.
- **A Data Generation Pipeline** (code name: /1D_Timoshenko_beam_data_generator/)
  - To automatically produce synthetic datasets for NN training.
- **Fully Connected Network** (code name: /1DFCN_medium_training/ and /1DFCN_full_training/)
- **1D Convolutional Neural Network** (code name: /1DCNN_medium_training/ and /1DCNN_full_training/)

---

## **Key Findings & Open Questions**
This is the core of my learning. Using the tool above, I trained simple neural networks to map modal data to damage locations.

- **The Experiment**: I compared a Fully Connected Network (FCN) and a 1D Convolutional Neural Network (CNN) on datasets of varying sizes.
- **The Observation**: An interesting pattern emerged:
  - On medium datasets (10k samples), the FCN achieved higher accuracy (82%).
  - On the full dataset (90k samples), the FCN’s performance dropped significantly (65%), while the CNN’s improved to become the better model (79%).
- **My Interpretation & Questions**:
  - This seems to highlight the challenge of **extreme class imbalance (damage points are rare)** and how different model architectures cope with it.
  - The CNN’s eventual superiority might align with the **localized nature of damage**, which matches its inductive bias. The FCN may have “overfitted” to the prevalent “no damage” class.
  - Most crucially, even the better CNN’s performance feels limited. This leads me to believe **the binary classification framework itself might be a bottleneck**, losing crucial information about damage severity and failing to express the inherent uncertainty of the inverse problem.

---

## **Future Directions & Invitation for Discussion**
My exploration suggests that improving simple models is not the main path forward. The more promising research directions appear to be:

- **Shifting the Problem Framing**: Moving from classification (“damage yes/no”) to continuous regression of the full stiffness field $E(x)$.
- **Embracing Uncertainty**: Exploring Bayesian methods to not only predict but also quantify the confidence of the prediction, which is vital for real engineering decisions.

I see this project as a starting point, not a conclusion. The framework I’ve built could serve as a testbed for these more advanced ideas. I am deeply curious about how to effectively implement them and would be eager to discuss this with potential advisors or collaborators.

---

## **Repository Structure**
```
/README/
/1D_Timoshenko_beam_FEM/
/1D_Timoshenko_beam_APDL/
/1D_Timoshenko_beam_KLexpansion/
/1D_Timoshenko_beam_mutation/
/1D_Timoshenko_beam_data_generator/
/1DFCN_meduim_training/
/1DFCN_full_training/
/1DCNN_meduim_training/
/1DCNN_full_training/
/Project Notes & Technical Documentation/ # Detailed technical notes (Chinese)
```
Notes: 
- Detailed theoretical derivations and implementation notes are provided in Chinese under /Project Notes & Technical Documentation/
- This repository is under active development and is intended for academic discussion and collaboration.

---

## **Epilogue**
As an undergraduate student, this project represents my passion for applying computational and data-driven methods to mechanical engineering problems. I am aware of its limitations and simplifications. Any feedback, suggestions for improvement, or pointers to related work would be immensely valuable and appreciated.
