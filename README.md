# QC-Club
This repository contains solution of Problem Statement 2.
The core goal is to determine ,with single query , if a given black-box function is constant(Beacon Theory) or balanced(Library Theory).
Given an oracle for a function f(x):
Constant: f(x) outputs the same value (0 or 1) for all x.
Balanced:f(x) outputs 0 for exactly half of the inputs and 1 for the other half.

A classical computer, in the worst case, would need 2^(N-1)+1 queries (where N=3) to definitively determine the function's nature. The Deutsch-Jozsa algorithm achieves this with just one query.

## Setup and Installation

To run the simulation, you need Python and the Qiskit quantum computing framework.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YourUsername/your-repo-name.git](https://github.com/YourUsername/your-repo-name.git)
    cd your-repo-name
    ```

2.  **Install dependencies:**
    ```bash
    pip install qiskit qiskit-aer numpy matplotlib jupyter
    ```

3.  **Run the script:**
    ```bash
    python deutsch_jozsa_simulation.ipynb
    # (Assuming you save the code as deutsch_jozsa_simulation.ipynb)
    ```
    ## 📐 Quantum Circuit Architecture

    Both simulations use a 4-qubit circuit:
    q1 , q2 : Qubits (q[0],q[1], q[2], q[3])
    ## General steps (Same for both)
    1. x on q[3] , then `h` on all q1 and q2 
    2. Oracle Uf | A single quantum query that applies a phase shift according to the function f(x)
    3. `h` on q[0], q[1], q[2]| The final Hadamard transform interferes the phases, mapping the result back to the computational basis. |
    4. Measure q[0], q[1], q[2]| The measurement reveals the function's property. |

# 🧪 Simulation Scenarios and Results

The script runs two distinct scenarios with 1711 shots each.

---

### SCENARIO 1: Constant Function ($f(x) = 1$)

* **Theory Analogy:** Beacon Theory
* **Oracle Implementation:** `qc1.x(q1[3])` (Since $q_3$ is already in $|-\rangle$, this applies a global phase shift of $-1$ across all states, leaving the final measurement result at $|000\rangle$).

**Result Interpretation:** If the function is constant, the input register will measure **$|000\rangle$ with 100% probability**.

#### Output:
SCENARIO 1: Beacon Theory (Constant Function)
Executing Quantum Query (1 time)...
The Beacon Theory is correct
{'0000': 1711}

*The key result is **'000'** in the input register.*

#### Circuit Visualization:

┌───┐┌──────────────────┐┌───┐ ░ ┌─┐      
q_0: ──┤ H ├┤0:              ├──┤ H ├─░─┤M├──────
├───┤│                │├───┤ ░ └╥┘|

q_1: ──┤ H ├┤1:              ├──┤ H ├─░─╫─┤M├────
├───┤│                │├───┤ ░ ║ └╥┘|

q_2: ──┤ H ├┤2:              ├──┤ H ├─░─╫──╫─┤M├─
┌─┴───┴┐│  preparation   │┌┴───┴┐ ░ ║  ║ └╥┘
q_3: ┤ X ├──┤3:              ├┤ X ├───░─╫──╫──╫──
└─────┘└────────────────┘└─────┘ ░ ║  ║  ║

c: 4/══════════════════════════════════╩══╩══╩════
0  1  2


---

### SCENARIO 2: Balanced Function ($f(x) = x_0 \oplus x_1 \oplus x_2$)

* **Theory Analogy:** Library Theory
* **Oracle Implementation:** `CX` gates from q[0], q[1], and q[2] to the target qubit q[3]. This implements the XOR sum, $\mathbf{f(x) = x_0 \oplus x_1 \oplus x_2}$.

**Result Interpretation:** If the function is balanced, the input register will measure **any state other than $|000\rangle$ with 100% probability**. For this specific function, the result should be $|111\rangle$.

#### Output:
SCENARIO 2: Library Theory (Balanced Function)
Executing Quantum Query (1 time)...
The Library Theory is correct
{'1110': 1711}

*The key result is **'111'** in the input register.*

#### Circuit Visualization:
   ┌───┐┌──────────────────┐     ┌───┐ ░ ┌─┐      
q_0: ──┤ H ├┤0:              ├──■──■─┤ H ├─░─┤M├──────
├───┤│                │  │  │ ├───┤ ░ └╥┘|

q_1: ──┤ H ├┤1:              ├──■──■─┤ H ├─░──╫─┤M├────
├───┤│                │  │  │ ├───┤ ░  ║ └╥┘|

q_2: ──┤ H ├┤2:              ├──■──■─┤ H ├─░──╫──╫─┤M├─
┌─┴───┴┐│  preparation   │┌┴───┴┐ ░ ┌┴───┴┐ ░ ║  ║ └╥┘
q_3: ┤ X ├──┤3:              ├┤ X ├──░─┤M├─
└─────┘└────────────────┘└─────┘ ░ ║  ║  ║
c: 4/═════════════════════════════════╩══╩══╩════
0  1  2


## 📝 Code Structure

The Python script is organized into two main sections, clearly defining the **Constant Case** and the **Balanced Case**, each following the four steps of the Deutsch-Jozsa algorithm.

* `qiskit` and `qiskit_aer` are used for circuit construction and simulation.
* `QuantumCircuit`, `QuantumRegister`, and `ClassicalRegister` manage the quantum resources.
* The `qasm_simulator` backend is used to run the circuits with `shots=1711`.

---

## 🤝 Contribution

Feel free to open issues or submit pull requests to improve the code, add more balanced function examples, or optimize the circuit construction!





