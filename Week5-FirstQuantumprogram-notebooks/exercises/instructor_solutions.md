# Lecture 5: Instructor Solutions

**Your First Quantum Program: From Qubits to Python**
Quantum for Energy Academy | Lecture 5 of 12

All numbers below come from actual runs with `StatevectorSampler(seed=...)`, Qiskit 2.6.1 and NumPy 2.5.4
(Python 3.12.13 and 3.13.18). With the same seed and the same package versions, students get the same counts.
With another seed, the counts differ, but every statement about probabilities stays true.

---

## Exercise 1: change the number of shots

**Reference change** in `src/hadamard_experiment.py`:

```python
SHOTS = 100    # then 1024, then 10000
SEED = 42
```

**Expected results (seed 42)**

| Shots | Count of 0 | Count of 1 | Frequency of 0 | Frequency of 1 |
|---:|---:|---:|---:|---:|
| 100 | 53 | 47 | 0.5300 | 0.4700 |
| 1,024 | 515 | 509 | 0.5029 | 0.4971 |
| 10,000 | 5,015 | 4,985 | 0.5015 | 0.4985 |

**Answers**

- a) Yes. Every shot gives exactly one outcome, so the counts always add up to the number of shots.
- b) In this run, 10,000 shots give the frequencies closest to 0.5. This is the typical case, but not a law for a single run (see Exercise 4).
- c) The counts change with the seed. The sum of the counts, the theoretical probabilities (0.5 and 0.5), and the circuit stay the same. Example: with `SEED = 7` and 100 shots, the result is 50 and 50, an exact half by chance.

---

## Exercise 2: replace H with X

**Reference change**

```python
circuit = QuantumCircuit(1, 1)
circuit.x(0)          # in place of circuit.h(0)
circuit.measure(0, 0)
```

**Prediction:** $X|0\rangle = |1\rangle$, so every shot gives 1.

**Expected result:** `{'1': 1024}` for 1,024 shots, and `{'1': 10}` for 10 shots, for every seed.

**Answers**

- a) No. After X, the state is the basis state $|1\rangle$ with $P(1) = |1|^2 = 1$. A measurement of a basis state is certain.
- b) Only the count changes: 10 shots give `{'1': 10}`. The result is still certain.

---

## Exercise 3: apply two H gates

**Reference change**

```python
circuit = QuantumCircuit(1, 1)
circuit.h(0)
circuit.h(0)          # second H
circuit.measure(0, 0)
```

**Prediction:** $HH = I$, so $HH|0\rangle = |0\rangle$ and every shot gives 0.

**Expected result:** `{'0': 1024}` for every seed.

**Mathematical explanation**

$$
HH|0\rangle = H\,\frac{|0\rangle + |1\rangle}{\sqrt{2}}
= \frac{H|0\rangle + H|1\rangle}{\sqrt{2}}
= \frac{1}{2}\Big[(|0\rangle + |1\rangle) + (|0\rangle - |1\rangle)\Big]
= |0\rangle .
$$

The two $|1\rangle$ terms have amplitudes $+\tfrac{1}{2}$ and $-\tfrac{1}{2}$, so they cancel (destructive interference).
The two $|0\rangle$ terms add up to amplitude 1 (constructive interference).
In matrix form,
$H^2 = \tfrac{1}{2}\begin{bmatrix}1 & 1\\ 1 & -1\end{bmatrix}\begin{bmatrix}1 & 1\\ 1 & -1\end{bmatrix} = \begin{bmatrix}1 & 0\\ 0 & 1\end{bmatrix} = I$.

**Experimental explanation:** all 1,024 shots give 0, so the measured frequency of 0 is exactly 1. The program
`src/double_h_experiment.py` also prints the state after one H, `[0.707, 0.707]`, and after two H gates, `[1, 0]`.

**Answer d)** A coin toss acts on probabilities, which are never negative, so two tosses can only mix the
outcomes further. The H gate acts on amplitudes, which can be negative. Amplitudes with opposite signs cancel.

---

## Exercise 4: measure the deviation from 0.5

**Expected results (seed 42)**, from `python src/shots_comparison.py`:

| Shots $N$ | $f_0$ | $\lvert f_0 - 0.5\rvert$ | $\sigma = 0.5/\sqrt{N}$ | Ratio |
|---:|---:|---:|---:|---:|
| 100 | 0.5300 | 0.0300 | 0.0500 | 0.60 |
| 1,024 | 0.5029 | 0.0029 | 0.0156 | 0.19 |
| 10,000 | 0.5015 | 0.0015 | 0.0050 | 0.30 |

**Answers**

- a) $\sigma$ shrinks by a factor of $\sqrt{100} = 10$: from 0.05 to 0.005. The spread falls like $1/\sqrt{N}$.
- b) Not always. Results of single runs:

  | Seed | $N = 100$ | $N = 1{,}024$ | $N = 10{,}000$ |
  |---:|---:|---:|---:|
  | 42 | 0.0300 | 0.0029 | 0.0015 |
  | 7 | 0.0000 | 0.0049 | 0.0017 |
  | 2026 | 0.0100 | 0.0371 | 0.0065 |

  With seed 7, the 100-shot run is an exact 50/50 split by chance. With seed 2026, the 1,024-shot run gives
  474 and 550, a deviation of 2.4 $\sigma$. Such a result occurs in about 2% of runs. What holds is the
  **average** behavior: the expected absolute deviation is about $\sqrt{2/\pi}\,\sigma \approx 0.8\,\sigma$,
  so it shrinks like $1/\sqrt{N}$. The test `test_deviation_shrinks_with_more_shots` checks this over 50 seeds.
- c) No. `StatevectorSampler` computes an ideal, noise-free state. All of the spread comes from the finite
  number of random samples. A real QPU adds gate errors, readout errors and decoherence on top.

**Reference code (any seed)**

```python
import math
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(1, 1)
qc.h(0)
qc.measure(0, 0)

for shots in (100, 1024, 10000):
    counts = StatevectorSampler(seed=42).run([qc], shots=shots).result()[0].data.c.get_counts()
    f0 = counts.get("0", 0) / shots
    sigma = 0.5 / math.sqrt(shots)
    print(f"{shots:6d}  f0 = {f0:.4f}  |f0 - 0.5| = {abs(f0 - 0.5):.4f}  sigma = {sigma:.4f}")
```

---

## Optional challenge: other gate sequences

**Expected results (seed 42, 1,024 shots)**

| Sequence | State before measurement | Counts |
|---|---|---|
| X, then H | $(\lvert 0\rangle - \lvert 1\rangle)/\sqrt{2}$ | `{'0': 515, '1': 509}` |
| H, then X | $(\lvert 0\rangle + \lvert 1\rangle)/\sqrt{2}$ | `{'0': 515, '1': 509}` |
| X, then H, then H | $\lvert 1\rangle$ | `{'1': 1024}` |
| X, then X | $\lvert 0\rangle$ | `{'0': 1024}` |
| H, then Z, then H | $\lvert 1\rangle$ | `{'1': 1024}` |

**Answers**

- a) No. "X, then H" gives $(|0\rangle - |1\rangle)/\sqrt{2}$, while "H" gives $(|0\rangle + |1\rangle)/\sqrt{2}$.
  The amplitudes differ in sign: `Statevector` shows `[0.707, -0.707]` and `[0.707, 0.707]`.
  The probabilities are equal, so the sampler draws from the same distribution. With the same seed it uses
  the same random numbers, so even the counts are identical.
- b) A histogram shows only the squared magnitudes of the amplitudes, measured in the computational basis.
  It hides the signs (relative phases). One more H reveals the difference: $H \cdot (|0\rangle - |1\rangle)/\sqrt{2} = |1\rangle$,
  while $H \cdot (|0\rangle + |1\rangle)/\sqrt{2} = |0\rangle$. The bonus sequence uses $HZH = X$.

**Reference code**

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1, 1)
qc.x(0)
qc.h(0)
print(Statevector(qc.remove_final_measurements(inplace=False)).data)  # [ 0.707+0.j -0.707+0.j]
qc.measure(0, 0)
print(StatevectorSampler(seed=42).run([qc], shots=1024).result()[0].data.c.get_counts())
```

---

## Quiz answer key

**Q1.** (b) 0.5. The amplitude is $1/\sqrt{2} \approx 0.707$; the probability is its square, $0.5$.

**Q2.** 2000. Each shot gives exactly one outcome.

**Q3.** (a) Yes. The deviation is $530 - 512 = 18$, about $1.1\,\sigma$ with $\sigma = \sqrt{1024 \cdot 0.5 \cdot 0.5} = 16$.
About 95% of runs lie between 480 and 544. (b) No. `StatevectorSampler` has no hardware noise. The deviation
comes from the finite number of random samples.

**Q4.** `{'0': 1024}`, with no error message at all. The classical bit exists but is never written, so it keeps
its start value 0. This silent mistake hides the H gate completely. (A circuit with no classical bit at all gives
an empty result, and Qiskit prints a short alert.)

**Q5.** Interference. Amplitudes carry signs, so the $|1\rangle$ amplitudes $+\tfrac{1}{2}$ and $-\tfrac{1}{2}$
cancel. Probabilities are never negative, so a process that only mixes probabilities cannot cancel an outcome.
