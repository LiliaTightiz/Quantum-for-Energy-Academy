# Quantum-for-Energy-Academy

## Learn Quantum. Build Energy Solutions.

Quantum for Energy Academy is an open educational initiative that connects quantum computing fundamentals with practical energy applications.

The program begins with simple quantum concepts and gradually progresses toward hands-on projects involving microgrids, renewable energy, battery energy storage systems (BESS), electric vehicles (EVs), and energy optimization.

Our Learning Ecosystem

Each weekly topic is presented through three connected platforms:

LinkedIn: Short explanations, discussions, and weekly updates
YouTube: Complete video lectures with examples
GitHub: Executable code, Jupyter notebooks, and small exercises
Platform	Content	Link
LinkedIn Group	Weekly explanations and community discussions	Join the group
YouTube	Complete video lessons and demonstrations	Watch the videos
GitHub	Code, notebooks, exercises, and project materials	You are here
Six-Month Learning Journey
Month 1: Quantum Foundations
Classical bits and qubits
Quantum states and amplitudes
Superposition
Measurement and shots
Bloch-sphere representation
Basic quantum gates
Month 2: Quantum Circuits and Coding
Qiskit fundamentals
Single-qubit circuits
Multi-qubit circuits
X, H, RY, and CNOT gates
Entanglement and interference
Noise and simulation basics
Month 3: Quantum Optimization
Binary optimization
Quadratic Unconstrained Binary Optimization (QUBO)
Ising models
Quantum Approximate Optimization Algorithm (QAOA)
Quantum annealing
Hybrid quantum-classical optimization
Month 4: Energy Problem Modeling
Microgrid operation
Renewable-energy integration
BESS scheduling
EV charging coordination
Power balance
Operational constraints
Month 5: Hands-On Energy Projects
Energy-cost minimization
Renewable-energy utilization
Battery scheduling
EV charging optimization
Constraint formulation
Hybrid solver implementation
Month 6: Capstone and Evaluation
Complete quantum-energy case study
Classical baseline implementation
Quantum and classical result comparison
Feasibility checking
Performance evaluation
Result visualization and interpretation
Week 1: Quantum Computing Fundamentals for Energy Systems

The first lecture introduces the fundamental concepts required for the rest of the program.

Topics
Energy scheduling challenges
Bits and qubits
Quantum states and probability amplitudes
Superposition
Measurement and repeated shots
X, H, RY, and CNOT gates
Interference and entanglement
Two-qubit energy-decision representation
Basic Qiskit circuit implementation
Difference between sampling and optimization
Energy Interpretation

In the introductory two-qubit example:

q0 represents the BESS charging decision.
q1 represents the EV charging decision.
0 means wait.
1 means charge.

Qiskit displays measurement keys in the order c1c0.

Qiskit result	Energy decision
00	Both wait
01	BESS charges
10	EV charges
11	Both charge

Important: Sampling candidate states is not equivalent to optimizing an energy schedule. Electricity cost, power balance, state of charge, EV requirements, and network limits must be mathematically encoded before an optimization method is applied.

Repository Organization

As the program develops, materials will be organized as follows:

Quantum-for-Energy-Academy/
│
├── week-01-quantum-fundamentals/
│   ├── notebooks/
│   ├── exercises/
│   └── README.md
│
├── week-02-quantum-gates/
├── week-03-multi-qubit-circuits/
├── qubo-energy-models/
├── qaoa-energy-projects/
├── datasets/
├── figures/
├── requirements.txt
└── README.md
Getting Started

Clone the repository:

git clone https://github.com/YOUR_USERNAME/Quantum-for-Energy-Academy.git
cd Quantum-for-Energy-Academy

Create a virtual environment:

python -m venv quantum-energy-env

Activate it on Windows:

quantum-energy-env\Scripts\activate

Activate it on macOS or Linux:

source quantum-energy-env/bin/activate

Install the initial packages:

pip install qiskit matplotlib jupyter

Start Jupyter Notebook:

jupyter notebook
Who Is This For?

This program is designed for:

Students beginning quantum computing
Energy and power-system researchers
Electrical and computer engineers
Data scientists interested in quantum optimization
Professionals exploring quantum-energy applications
Anyone interested in learning through practical examples

Prior knowledge of quantum physics is not required. Basic Python knowledge is helpful for the coding exercises.

Educational Principles
Begin with the fundamentals.
Explain concepts using simple language.
Connect quantum concepts to energy decisions.
Provide executable and understandable code.
Use small and reproducible examples.
Compare quantum methods with classical baselines.
Avoid unsupported claims of quantum advantage.
Validate every solution against physical energy constraints.
Contributions and Discussions

Questions, corrections, suggestions, and educational contributions are welcome.

You can participate by:

Joining the weekly LinkedIn discussions
Commenting on the YouTube lectures
Opening a GitHub issue
Suggesting an energy problem for a future project
Improving documentation or exercises
Disclaimer

This repository is intended for education and research. Introductory circuits demonstrate quantum representation and sampling. They do not establish quantum advantage or automatically produce optimal energy schedules.

Connect With the Academy
LinkedIn Group: linkedin.com/groups/38560002
YouTube Channel: www.youtube.com/@QuantumEnergyAcadem
GitHub Repository
