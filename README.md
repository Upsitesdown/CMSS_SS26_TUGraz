# CMSS_SS26_TUGraz

**Final Project of the course "Computational Modelling of Social Systems" at TU Graz**

## Overview

This project investigates whether **self-organized homophily** (the tendency of similar people to connect with each other) makes misinformation more contagious through a computational agent-based model (ABM).

The model combines two key mechanisms:
- **Network rewiring**: Agents dynamically rewire their social ties toward more similar others (inspired by Smirnov & Thurner 2017)
- **Misinformation spreading**: A belief/fact-checking epidemic process runs simultaneously on the rewiring network (inspired by Tambuscio et al. 2015)

The research question is answered quantitatively: *Does a higher proportion of the population end up believing misinformation when the network becomes more homophilic?*

## Research Question & Hypotheses

**Main Question**: Does self-organized homophily make misinformation more contagious?

**Hypotheses**:
1. **H1 (Main effect)**: Lower homophily resistance (strict preference for similar neighbors) leads to higher misinformation spread
2. **H2 (Threshold shift)**: Homophilic clustering raises the fact-checking effort needed to eradicate misinformation
3. **H3 (Non-linearity)**: The relationship between network homophily and misinformation spread is non-linear

## Project Structure

- **CMSS_Project(3).ipynb** - Main Jupyter notebook containing:
  - Literature review and theoretical background
  - Implementation of `MisinfoAgent` and `HomophilyMisinfoModel` classes
  - Diagnostic runs and validation
  - Theta sweep experiment (tests H1 & H3)
  - Threshold-shift experiment (tests H2)
  - Robustness checks across different network topologies and initial conditions
  - Visualizations of results

- **Report Computational Modelling of Social Systems.pdf** - Written report with detailed findings

## Requirements

- Python 3.8+
- **Key packages**:
  - `mesa` (≥3.5) - Agent-based modeling framework
  - `networkx` - Network/graph operations
  - `numpy` - Numerical computing
  - `matplotlib` - Visualization
  - `pandas` - Data analysis
  - `tqdm` - Progress bars

## Installation & Setup

1. **Clone or download the repository**

2. **Install dependencies**:
   ```bash
   pip install "mesa[rec]" networkx numpy matplotlib pandas tqdm
   ```

3. **Run the notebook**:
   - Open `CMSS_Project.ipynb` in Jupyter
   - Execute cells sequentially, starting from Section A (imports)
   - The notebook is fully reproducible with a fixed global random seed

## How It Works

### The Model

**Agents** (`MisinfoAgent`):
- Each agent has a fixed **trait** (0–1, static homophily dimension)
- Each agent has a dynamic **SBFC state**: Susceptible (S), Believer (B), or Fact-Checker (F)

**Network**:
- Base graph: Erdős–Rényi or Barabási–Albert
- Agents dynamically rewire ties toward more similar others (selection mechanism)

**Dynamics** (each simulation step):
1. **Rewiring phase**: Each agent attempts one homophilic rewire (Smirnov & Thurner 2017 rule)
   - Pick an existing tie and a potential new contact
   - If the new contact is more similar, rewire; otherwise do so randomly with probability θ
2. **Spreading phase**: SBFC transitions (Tambuscio et al. 2015)
   - Susceptibles can become Believers or Fact-Checkers based on neighbor influence
   - Believers can fact-check and become Fact-Checkers
   - Any agent can forget and return to Susceptible
3. **Data collection**: Record state fractions and homophily index

### Key Parameters

- **θ** (theta): Rewiring randomness (0 = strict homophily, 1 = random rewiring)
- **β** (beta): Spreading rate (how often content reaches a neighbor)
- **α** (alpha): Credibility bias (advantage for misinformation over debunking)
- **p_verify**: Probability a Believer fact-checks per step
- **p_forget**: Probability any agent forgets per step

## Running Experiments

The notebook contains three main experiments:

1. **Diagnostic Run** (Section C): Single run to validate model behavior
2. **Theta Sweep** (Section D): Varies homophily (θ) across 0–1, 30 seeds each
   - Tests whether stricter homophily increases misinformation spread
3. **Threshold Shift** (Section E): Sweeps fact-checking effort at extreme θ values
   - Tests whether homophily changes the eradication threshold
4. **Robustness Checks** (Section F): Tests across network types and initial conditions

## Key Findings

- **H1 rejected**: Strict homophily (θ=0) actually *suppresses* misinformation (mean pB=0.15), contrary to expectation
- **H2 rejected**: The eradication threshold does not shift with homophily level
- **H3 weak support**: A small non-linear rise from θ=0 to θ≈0.3, then plateau

**Interpretation**: Self-organized homophily in this model acts as a *containment mechanism* rather than an amplifier. Believers cluster together but become isolated from susceptibles, limiting spread.
