# Lecture 5: Student Exercises

**Your First Quantum Program: From Qubits to Python**
Quantum for Energy Academy | Lecture 5 of 12

Work in the activated virtual environment, from the folder `lectures/L05_First_Quantum_Program`.
Each exercise names the file to edit. Write down your prediction **before** you run any code.
The solutions are in `instructor_solutions.md`. Please try on your own first.

| Exercise | Level | File | Time |
|---|---|---|---|
| 1. Change the number of shots | Basic | `src/hadamard_experiment.py` | 10 min |
| 2. Replace H with X | Basic | `src/hadamard_experiment.py` | 10 min |
| 3. Apply two H gates | Intermediate | `src/hadamard_experiment.py` | 15 min |
| 4. Measure the deviation from 0.5 | Intermediate | `src/shots_comparison.py` | 20 min |
| Optional challenge: other gate sequences | Advanced | `notebooks/02_Quantum_Gates.ipynb` | 20 min |
| Quiz: five conceptual questions | All | this sheet | 10 min |

---

## Exercise 1 (basic): change the number of shots

In `src/hadamard_experiment.py`, the constant `SHOTS` sets the number of repetitions.

1. Run the program with `SHOTS = 1024` and write down the counts and the frequencies.
2. Change `SHOTS` to `100`, then to `10000`. Run the program each time.
3. Fill in the table.

| Shots | Count of 0 | Count of 1 | Frequency of 0 | Frequency of 1 |
|---:|---:|---:|---:|---:|
| 100 | | | | |
| 1,024 | | | | |
| 10,000 | | | | |

**Questions**

- a) Do the counts always add up to the number of shots?
- b) Which run gives frequencies closest to the theoretical probability 0.5?
- c) Change `SEED = 42` to another value and run again with 100 shots. What changes, and what stays the same?

---

## Exercise 2 (basic): replace H with X

In `build_hadamard_circuit`, replace the line `circuit.h(0)` with `circuit.x(0)`.

1. **Predict first.** Write down the counts that you expect for 1,024 shots. Use $X|0\rangle$.
2. Run the program and compare the result with your prediction.

**Questions**

- a) Is the result random? Explain with the state after the gate.
- b) What changes if you use 10 shots instead of 1,024?

---

## Exercise 3 (intermediate): apply two H gates

Restore `circuit.h(0)` and add a second `circuit.h(0)` right after it (before the measurement).

1. **Predict first.** Write down the counts that you expect for 1,024 shots.
2. Run the program.
3. Explain the result **mathematically**: compute $HH|0\rangle$ step by step. Use $H|0\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$ and $H|1\rangle = (|0\rangle - |1\rangle)/\sqrt{2}$.
4. Explain the result **experimentally**: what do the counts show?

**Question**

- d) Two fair coin tosses never undo each other. Why do two H gates undo each other?

---

## Exercise 4 (intermediate): measure the deviation from 0.5

Run `src/shots_comparison.py`. It runs the H circuit with 100, 1,024 and 10,000 shots.

1. For each number of shots $N$, compute the absolute deviation $|f_0 - 0.5|$, where $f_0$ is the frequency of outcome 0.
2. Compute the standard deviation of the frequency, $\sigma = \sqrt{p(1-p)/N} = 0.5/\sqrt{N}$, for each $N$.
3. Fill in the table.

| Shots $N$ | $f_0$ | $\lvert f_0 - 0.5\rvert$ | $\sigma = 0.5/\sqrt{N}$ | Ratio $\lvert f_0 - 0.5\rvert / \sigma$ |
|---:|---:|---:|---:|---:|
| 100 | | | | |
| 1,024 | | | | |
| 10,000 | | | | |

**Questions**

- a) How does $\sigma$ change when $N$ grows by a factor of 100?
- b) Change `SEED` to 7 and then to 2026. Does the deviation **always** shrink from 100 to 1,024 to 10,000 shots in a single run? What does hold on average?
- c) Is any of the deviations caused by hardware noise? Explain.

---

## Optional challenge: other gate sequences

Use the helper `run_counts` in notebook `02_Quantum_Gates.ipynb` (section 5), or edit a copy of a script.
For each sequence, **predict** the counts for 1,024 shots, then check.

| Sequence (left to right) | Your prediction | Result |
|---|---|---|
| X, then H | | |
| H, then X | | |
| X, then H, then H | | |
| X, then X | | |
| Bonus: H, then Z, then H | | |

The Z gate is $Z = \begin{bmatrix}1 & 0\\ 0 & -1\end{bmatrix}$. In Qiskit: `qc.z(0)`.

**Questions**

- a) "X, then H" and "H" give the same histogram. Are the two states the same? Hint: compare `Statevector(...)` of both circuits.
- b) What does a histogram show, and what does it hide?

---

## Quiz: five conceptual questions

**Q1.** A qubit is in the state $(|0\rangle + |1\rangle)/\sqrt{2}$. What is the probability of the outcome 1?

- (a) 0.707  (b) 0.5  (c) 1  (d) 0

**Q2.** A circuit runs with `shots=2000`. What is the sum of all counts in the result?

**Q3.** You run the H circuit with 1,024 shots and get `{'0': 530, '1': 494}`.

- (a) Is this result consistent with the theory?
- (b) Is the difference from 512 caused by hardware noise?

**Q4.** Your circuit is `QuantumCircuit(1, 1)` with `qc.h(0)`, but there is no `measure` line. What does `StatevectorSampler` return for 1,024 shots?

**Q5.** After one H gate the outcome is random. After two H gates the outcome is always 0. Which effect explains this, and why can it not happen with probabilities alone?
