

# ⏱️ Stop-Clock 


## 🎯 Objective

To design and implement a digital stopwatch with the following features:

- A **4-digit display** that shows elapsed time in the format `SS.HH`, where `SS` represents seconds and `HH` represents hundredths of a second (e.g., `23.78` indicates 23.78 seconds).
- A **single push-button interface** that cycles through three operational states: **RESET**, **START**, and **STOP**, in a sequential loop.

### Functional States:
- **RESET**: Initializes the stopwatch to `00.00`, preparing it to begin timing.
- **START**: Begins counting in 0.01-second increments. The display updates in real-time until the button is pressed again.
- **STOP**: Freezes the current time on the display. Pressing the button once more resets the stopwatch and returns it to the initial state.

### 🔄 State Machine Logic

```plaintext
[RESET] --(Button Press)--> [START]
[START] --(Button Press)--> [STOP]
[STOP]  --(Button Press)--> [RESET]
```


## 🛠️ System Components

### 1. **Clock Generator (10 ms Pulse)**
- **IC Used**: A 555 timer is an integrated circuit used to generate precise time delays or oscillations, commonly configured three modes such as astable, monostable,and bistable modes.
- **Configuration**:In astable mode, the 555 timer functions as a free-running oscillator—it continuously switches between high and low output states without any external triggering. This makes it ideal for generating clock pulses, hence we used it for generating  10 ms clock used in our project.
- **LT Spice simulation**:[Circuit](https://github.com/Manvi1670/STOP-CLOCK/blob/40ddc953ea36c8df6870716ed4e70b379bcfeef0/Timer%20-%20555%20Lt%20Spice%20simulation.png) 


### 2. **Debouncing Circuit**
- **Purpose**: Mechanical switches generate noisy signals due to contact bounce, which can cause multiple false triggers in digital circuits. Debouncing ensures a clean,        single transition per press, making systems like stopclocks or counters behave reliably.
- **Circuit**: The RC network smooths out the rapid voltage fluctuations caused by contact bounce, and the Schmitt trigger converts this analog signal into a clean digital transition with hysteresis, ensuring reliable state changes in the stopclock.
- **Simulation**: Verified using LTSpice to ensure clean, single-edge transitions.

### 3.Counters
- **ICs Used**: 74160 , 74161

## 🔍 IC 74160 vs IC 74161

| Feature                  | **IC 74160**                                | **IC 74161**                                |
|--------------------------|---------------------------------------------|---------------------------------------------|
| **Type**                 | BCD (Binary-Coded Decimal) Counter          | Binary Counter                              |
| **Count Range**          | 0 to 9 (0000 to 1001 in binary)             | 0 to 15 (0000 to 1111 in binary)            |
| **Clear Type**           | Asynchronous Clear                          | Asynchronous Clear                          |
| **Load Type**            | Synchronous Load                            | Synchronous Load                            |
| **Enable Inputs**        | ENP and ENT                                 | ENP and ENT                                 |
| **Ripple Carry Output**  | HIGH at count 9 if ENT is HIGH              | HIGH at count 15 if ENT is HIGH             |
| **Clock Trigger**        | Rising edge                                 | Rising edge                                 |


---

## 🔷 7447 - Decoder

The **7447** is a decoder IC that converts a **4-bit BCD input** into signals to drive a **common-anode 7-segment display**, showing digits **0 to 9**.

### Key Points:
- **Inputs**: A, B, C, D (BCD from counter)
- **Outputs**: a–g (active-low segment controls)
- **Used with**: Common-anode displays

### 🔗 Connection to 74160:
- The **QA–QD outputs** of each 74160 BCD counter are connected to the **A–D inputs** of a 7447.
- Each 7447 then drives one **7-segment display**, showing the corresponding digit.

This setup allows your stopwatch to visually display the count from each 74160 stage.




## 🔷 7-Segment Common Anode Display

A **7-segment common anode display** has all anodes connected to **Vcc**, and each segment (a–g) lights up when its **cathode is pulled LOW**.

### Key Points:
- Displays digits 0–9 using 7 LEDs
- Works with **7447 decoder IC**, which provides active-low outputs
- Each **7447** receives BCD input from a **74160 counter** and drives one display

This setup allows your stopwatch to show each digit clearly and efficiently.


## Practical implementation Photos:
[Entire Circuit](https://github.com/Veda1906/Stop_Clock/blob/main/Whole%20circuit.jpeg)
[Clock Generator with 555 timer](https://github.com/Veda1906/Stop_Clock/blob/main/555%20timer.jpeg)
