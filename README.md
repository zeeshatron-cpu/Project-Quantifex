Project Quantifex
Machine Optimization of Quantum Algorithms on Noisy Hardware

🥇 1st Place — WSSEF Science Fair 2026

Quantum circuits degrade under real-world noise, making reliable computation on today's NISQ-era hardware a fundamental challenge. This project tests whether Bayesian Optimization (BO) can automatically discover QFT/AQFT circuit configurations that outperform both random search and human-guided tuning — using IBM's real hardware noise models.

Results
MethodMean Score95% CIBayesian Optimization0.6950±0.0022Random Search0.6752±0.0085Human Heuristic0.6754±0.0070
Bayesian Optimization achieved the highest mean score, fastest convergence, and lowest regret across all trials. The best circuit found was an AQFT with k=1, opt_level=3, t_count=5, r=1, layout=sabre, routing=sabre — scoring 0.7104 (vs. best human circuit at 0.662).

What This Project Does
Standard QFT circuits have several tunable parameters that significantly affect performance under noise:

Circuit type — exact QFT vs. Approximate QFT (AQFT) with truncation level k
Optimization level — Qiskit transpiler optimization (0–3)
Repetitions — number of controlled-phase gate repetitions
Layout & routing strategy — sabre, trivial, etc.

Manually tuning these is slow and unreliable. This project frames it as a black-box optimization problem and applies Bayesian Optimization with a Gaussian Process surrogate model to search the parameter space efficiently.
Score Formula
score = success_probability − 0.015 × depth − 0.0005 × CNOT_count
This penalizes circuit depth and gate count, rewarding both accuracy and hardware efficiency.

Pipeline Overview
Build QFT/AQFT Circuits
        ↓
Apply IBM Noise Simulator (ibm_fez noise model)
        ↓
Evaluate Performance (score, depth, CNOT count)
        ↓
Bayesian Optimization loop (Gaussian Process + Upper Confidence Bound)
        ↓
Compare vs. Random Search + Human Heuristic
        ↓
Statistical Analysis (t-tests, 95% CI, effect size)

Tech Stack
ToolPurposeQiskitBuild and simulate quantum circuitsqiskit-aerIBM noise model simulation (ibm_fez)scikit-learnGaussian Process Regressor (Matérn kernel)scikit-optimizeBayesian Optimization frameworkNumPy / SciPyNumerical computation + statistical testingMatplotlibConvergence curves, regret plots, heatmapspandasResults tracking and analysis

Repo Structure
quantifex/
├── Quantifex_clean.ipynb   # Full research pipeline (token-safe)
├── requirements.txt        # Dependencies
├── 
│   └── poster.png          # Science fair poster
├── results/                # Output directory (auto-generated on run)
│   ├── main_experiment_results.json
│   ├── trials.csv
│   └── figures/
└── README.md

Quickstart
bash# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/quantifex.git
cd quantifex

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set your IBM Quantum token (get one free at quantum.ibm.com)
export IBM_QUANTUM_TOKEN="your_token_here"

# 4. Open the notebook
jupyter notebook Quantifex_clean.ipynb

No IBM account? Set USE_IBM_NOISE_MODEL = False in CONFIG to run with the built-in synthetic NISQ noise model. Results will differ slightly but the pipeline runs fully offline.


Key Findings

Bayesian Optimization outperformed random search and human-guided tuning across 10 independent seeds
BO showed faster convergence — finding strong configurations within the first 20 trials
BO had lower regret, meaning fewer wasted evaluations on poor configurations
The best circuit balanced accuracy with lower depth and gate count under noise
Results suggest machine-guided search can meaningfully improve the practicality of quantum algorithms on today's noisy hardware


Limitations & Future Work

Noise model was simulated (ibm_fez) rather than live hardware execution
Search space was discrete and constrained — continuous relaxation may yield better optima
Future: test on real quantum hardware, expand to other algorithms (Grover's, QPE), increase evaluation budget


Background
This project builds on:

Hui, J. (2018). Quantum Fourier Transform
Johnson et al. (2014). What is a Quantum Simulator?
Quantum Inspire. CNOT Gate
AWS Quantum Technologies Blog. Noise in Quantum Computing
Makone, A. Exploration vs Exploitation


Built with Qiskit + scikit-learn. Noise model sourced from IBM Quantum (ibm_fez). Science fair project — WSSEF 2026.
