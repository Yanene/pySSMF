---
title: 'pySSMF: A Python package for slave spin mean field method'
tags:
  - Python
  - Condensed matter Physics
  - Strongly correlated electron systems
  - Hubbard model
  - slave spin mean field method
authors:
  - name: Youssra. Anene
    orcid: 'https://orcid.org/0009-0008-3622-9196'
    #equal-contrib: true
    corresponding: true 
    affiliation: "1" # (Multiple affiliations must be quoted)
  - name: José M. Pizarro
    orcid: 'https://orcid.org/0000-0002-6751-8192'
    affiliation: 2
  - name: Lorenzo. Fratino
    orcid: 'https://orcid.org/0000-0003-1288-2859'
    affiliation: 1, 3
affiliations:
 - name: LPTM, CNRS UMR 8089, CY Cergy Paris University, France
   index: 1
 - name: Federal Institute for Materials Research and Testing (BAM), Germany
   index: 2
 - name: Lille University, CNRS Centrale Lille, Université Polytechnique Hauts de France, Junia-ISEN,UMR 8520, IEMN, France
   index: 3 
date: 21 May 2026
bibliography: paper.bib
#csl: ieee.csl

---

# Summary
....

# Statement of need

Quantum materials such as high-temperature superconductors, Mott insulators, and heavy-fermion compounds exhibit behavior that single-particle tight-binding models cannot capture, because they neglect the electron-electron correlations that govern these systems. Realistic modeling therefore requires renormalized band structures that encode many-body effects. Methods such as DFT+DMFT provide these, but their computational cost makes them impractical for high-throughput screening or broad parameter-space exploration.
Slave-spin mean-field (SSMF) theory [@de2005orbital;@georges2013strong;@de2017modeling;@crispino2023slave] offers an inexpensive alternative. It captures correlation-driven phenomena, including Mott transitions, quasiparticle weight renormalization, and magnetic order, at a fraction of the cost of DMFT. However, no open-source, standardized implementation exists. Existing codes are largely confined to individual research groups, poorly documented, and not designed for systematic or reproducible calculations, which hinders validation, benchmarking, and extension of the method.
`pySSMF` fills this gap. It is a Python package implementing SSMF for the two-dimensional Hubbard model, supporting single- and multi-orbital systems, paramagnetic and antiferromagnetic phases, and multiple lattice geometries. Its interface is built for efficient parameter scans, enabling rapid computation of quasiparticle weights across large regions of model space. By providing a documented, reproducible implementation, `pySSMF` lets condensed matter physicists and adjacent researchers study strongly correlated systems without the overhead of full many-body frameworks.



# State of the field    

Strongly correlated electron systems are usually studied with dynamical mean-field theory (DMFT) and its extensions, such as cluster DMFT and GW+EDMFT. These methods describe multiorbital Hubbard physics with high fidelity and are supported by mature open-source frameworks, including `TRIQS` [@TRIQS2015] and `w2dynamics` [@WALLERBERGER2019388]. Their computational cost, however, limits systematic exploration of parameter space. Downfolding and low-energy effective models reduce this cost, but they typically still rely on a DMFT solver at the final step.
Slave-spin mean-field (SSMF) theory [@de2005orbital; @de2017modeling] is a lightweight alternative. It reproduces Mott transition and captures  Néel antiferromagnetic order at a fraction of the computational cost of DMFT[@crispino2023slave]. To our knowledge, no open-source implementation of SSMF exists. Published calculations rely on in-house codes that are not documented for external use, which makes results hard to reproduce and compare across studies.
We therefore built `pySSMF` as a standalone package instead of extending an existing DMFT framework. DMFT maps the lattice problem onto a quantum impurity problem that requires a dedicated impurity solver, whereas SSMF treats the lattice problem directly through a mean-field decoupling of fermionic and slave-spin degrees of freedom. The two approaches therefore share little computational infrastructure.


# Software design
## Theoretical background
`pySSMF` solves the multi-orbital Hubbard model on a lattice,
$$
H = \sum_{ij,m,\sigma} t_{ij}\, d^{\dagger}_{im\sigma} d_{jm\sigma}- \mu \sum_{i,m,\sigma} n_{im\sigma} + H_{int},
$$
where $d_{im\sigma}^\dagger$ creates an electron at site $i$, orbital $m$, spin $\sigma$, and $H_{int}$ contains the intra- and inter-orbital Coulomb repulsions $U$ and $U'=U-2J$ and the Hund's coupling $J$. In the $Z_2$ slave-spin representation [@de2005orbital;@de2017modeling], each operator is written as 
$d_{im\sigma} \rightarrow f_{im\sigma}\, O_{im\sigma}, \quad
d^{\dagger}_{im\sigma} \rightarrow O^{\dagger}_{im\sigma} f^{\dagger}_{im\sigma}$, with an auxialliary fermion $f_{im\sigma}$ and a spin-1/2 operator $O_{im\sigma}$
built from slave spins. The physical subspace is enforced by the constraint
$$
f^{\dagger}_{im\sigma} f_{im\sigma} = S^z_{im\sigma} + \frac{1}{2},
$$
A mean-field decoupling separates the problem into two sectors. The fermions move in a tight-binding model whose hopping is renormalized by slave-spin expectation values, which yields the quasiparticle weight $Z_{m\sigma}=\langle O_{im\sigma}\rangle^2$. The slave spins obey a single-site problem that is solved exaclty. The two sectors are iterated to self-consistency, and antiferromagnetic order is reached through a sublattice extension [@crispino2023slave]. This loop is the core of the package (see next subsection). # link to the full mathematical derivation!!!
## Architecture and workflow
Architecture + self-consistency loop + workflow image
# Research impact statement

`pySSMF` makes slave-spin mean-field calculations reproducible and openly available, and it supports both paramagnetic and antiferromagnetic phases. Its results have been validated against published slave-spin and DMFT results: it reproduces (specific quantities), e.g. the quasiparticle weight versus interaction strength and so we can extrat the critical interaction value where the Mott transition occurs, the interaction-dependence of the quasiparticle weight from [@de2005orbital] also of the double occupancy, also the energies and other quantities, and staggered magnetization for in the antiferromagnetic phase of [@crispino2023slave]. 

To add also:
 - Benchmark figures/ evidence:  distributed with the package as datasets and Jupyter notebook tutorials +  checked by an automated test suite --> users can verify the implementation and reproduce the figures independently
 - package release on PyPI --> facilitate installation and distribution
 - Archiving and citation: Zenodo --> DOI for reproducibility and citation
 - URL Documentation

Planned near-term extensions include support for tight-binding models derived from real materials, so that the same workflow can be applied beyond model Hamiltonians.# 


# AI usage disclosure

AI tools, including Codex, were used in a limited way to support brainstorming of software design ideas. 
No AI tools were used for implementation, data analysis, or scientific interpretation. The code and results were fully developed, tested, and validated by the authors.
 

# References
