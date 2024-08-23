### README: Superdense Coding with Cirq

### Project Overview

This project demonstrates the implementation of **Superdense Coding** using the Cirq quantum computing framework. Superdense coding is a quantum communication protocol that allows the transmission of two classical bits of information using just one qubit, provided that the qubit is entangled with another qubit shared between two parties, Alice and Bob. This implementation showcases the following key steps:

1. **Creating a Bell State**: Generating an entangled state shared between Alice and Bob.
2. **Encoding the Message**: Alice encodes a two-bit classical message onto her qubit using quantum gates.
3. **Sending the Qubit**: Alice sends her qubit to Bob.
4. **Decoding the Message**: Bob applies a series of quantum gates to retrieve the original two-bit message.

---

### Code Walkthrough

#### 1. **Bell State Generation**

The function `Bell()` is defined to create a Bell state, which is an entangled quantum state of two qubits. Specifically, the state generated is:

\[
\frac{1}{\sqrt{2}} \left( |00\rangle + |11\rangle \right)
\]

```python
def Bell():
    circuito = cirq.Circuit()
    a, b = cirq.LineQubit.range(2)
    circuito.append(cirq.H(a))  # Apply Hadamard gate on qubit 'a'
    circuito.append(cirq.CNOT(a, b))  # Apply CNOT gate with 'a' as control and 'b' as target
    return a, b, circuito
```

#### 2. **Circuit and Simulation Results Display**

The function `stampaCircuito()` is defined to print the quantum circuit and simulate its results using Cirq's simulator.

```python
def stampaCircuito(circuito, simulatore):
    print('Circuito:')
    risultati = simulatore.simulate(circuito)
    print(risultati)
    print('\n\n')
    print(circuito)
    print('\n')
```

#### 3. **Measurement and Histogram**

The function `misura()` is defined to perform a measurement on the qubits and plot a histogram to display the results.

```python
def misura(simulatore, a, b, circuito):
    n = 1000
    circuito.append(cirq.measure(a, b, key='risultato'))
    campioni = simulatore.run(circuito, repetitions=n)  # Run the circuit 'n' times
    conteggi = campioni.histogram(key='risultato')
    cirq.plot_state_histogram(conteggi, plt.subplot())
    plt.show()
```

#### 4. **Message Encoding and Decoding**

A dictionary `messaggio` is defined to map classical two-bit messages to the corresponding quantum operations that Alice should perform on her qubit.

```python
messaggio = {
    "00": [],
    "01": [cirq.X(a)],
    "10": [cirq.Z(a)],
    "11": [cirq.Z(a), cirq.X(a)]
}
```

Alice chooses a message to send and applies the corresponding operations to her qubit.

```python
m = "00"  # Change this to any message you'd like to send
print("Alice manda = ", m)
circuito.append(messaggio[m])
```

Bob then applies the necessary gates to decode the message:

```python
circuito.append(cirq.CNOT(a, b))
circuito.append(cirq.H(a))
stampaCircuito(circuito, s)
misura(s, a, b, circuito)
```

#### 5. **Results Interpretation**

After executing the circuit and measuring the qubits, Bob will retrieve the original message sent by Alice. The output of the simulation confirms the successful retrieval of the message.

For example, if Alice encodes the message `"11"`, Bob should receive the state \(|11\rangle\) upon decoding, which corresponds to the classical bit string `11`.

---

### How to Run the Code

1. **Dependencies**: Ensure you have Python installed along with the required libraries:
   - Cirq
   - Matplotlib

   You can install these using pip:
   ```bash
   pip install cirq matplotlib
   ```

2. **Running the Script**: Simply run the Python script in your terminal or an IDE that supports Python. The results of the circuit and the measurement histogram will be displayed.

3. **Customizing the Message**: To send a different message, modify the `m` variable in the script to any of `"00"`, `"01"`, `"10"`, or `"11"`.

---

### Conclusion

This project demonstrates the fundamental principles of quantum communication using superdense coding. By leveraging quantum entanglement, we can achieve more efficient communication than classical means allow, transmitting two classical bits using just one qubit.

---

### References

- [Cirq Documentation](https://quantumai.google/cirq)
- [Quantum Computing: A Gentle Introduction](https://mitpress.mit.edu/9780262535435/quantum-computing/)

---
