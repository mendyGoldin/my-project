# Project Proposal

**Project Title: Agentic Analog Filter Design: Automated Filter-Type and Topology Selection, Component Optimization, and SPICE Verification**

## 1. The problem and goal

**The problem and goal in my project:**

Designing analog filters for sensor interfaces, measurement systems, and signal-conditioning applications requires translating performance requirements into practical electronic circuits. These circuits must preserve useful signal frequencies while attenuating unwanted components, subject to constraints such as cutoff frequency, passband flatness, stopband attenuation, circuit gain, component availability, and operational amplifier limitations.

A central challenge is selecting an appropriate filter type and circuit topology. Depending on the application, the required filter may be a low-pass filter, which attenuates high-frequency signals, or a high-pass filter, which attenuates low-frequency signals. Different circuit architectures, such as Sallen-Key and Multiple Feedback (MFB), offer different trade-offs in terms of component count, gain and polarity, sensitivity, and practical performance.

Once the architecture is selected, suitable resistor and capacitor values must be determined. Analytical equations provide ideal continuous component values, while practical implementation is constrained by commercially available components, such as the E12 standard series. Furthermore, non-ideal operational amplifier characteristics, particularly finite gain-bandwidth product (GBW), can cause deviations from the theoretical filter response.

**The goal of this project is to develop and evaluate an LLM-based engineering agent that autonomously coordinates the design and simulation-based verification of analog filters from high-level engineering specifications.**

The agent will determine the required filter type, select a suitable circuit topology, coordinate component calculations and optimization, and evaluate candidate designs using circuit simulation. When a design fails to meet its requirements, the agent will diagnose the results and revise its approach.

The initial implementation will support second-order low-pass and high-pass filters using Sallen-Key and MFB topologies. Designs will use E12 resistor and capacitor values within predefined practical ranges and a fixed operational amplifier model.

Performance will be evaluated through specification satisfaction, frequency-response accuracy, stopband attenuation, circuit complexity, simulation efficiency, and failure-diagnosis capability. The system will also identify cases where no satisfactory design is found within the supported architectures and search budget.

For example, a measurement system may require a filter that preserves signals up to 1 kHz while attenuating interference at 10 kHz by at least 35 dB. The agent must determine the appropriate filter type and circuit topology, select commercially available components, and verify through simulation whether the resulting design satisfies these requirements.

## 2. The simulator

**The simulator in my project:**

The project will use **ngspice**, an open-source analog circuit simulator, as its deterministic simulation environment.

Candidate circuits will be generated from a library of predefined, parameterized SPICE netlist templates representing the supported filter types and topologies. A Python interface will translate structured design configurations into simulation inputs, execute ngspice, and extract the resulting performance metrics.

The system will accept engineering specifications through structured text-based inputs, such as JSON. These specifications will describe the desired frequency response, gain requirements, component constraints, and relevant hardware limitations.

The simulator will receive circuit configurations containing the selected topology, resistor and capacitor values, operational amplifier parameters, supply voltages, and frequency-sweep settings.

It will perform AC frequency-response analysis and DC operating-point analysis.

The main outputs will include:

- Frequency-response magnitude and phase.
- Measured cutoff frequency and passband characteristics.
- Attenuation at specified stopband frequencies.
- DC operating-point information.
- Simulation execution status, warnings, and diagnostic logs.

Performance metrics will be calculated by a deterministic evaluation layer using measurement rules appropriate to the selected filter type. In particular, nominal gain and frequency-dependent attenuation will be evaluated separately against the specified requirements.

ngspice can run locally on Linux or Windows through WSL and supports command-line execution without a graphical interface. It can also be integrated into automated tests and GitHub Actions for reproducible verification.

Representative circuit implementations will first be checked manually using LTspice before being incorporated into the automated ngspice workflow.

## 3. The agent's role

**The agent in my project:**

The LLM-based agent will act as an engineering design coordinator, managing decisions across circuit architecture selection, component design, simulation, and verification.

Its responsibilities will include:

**1. Specification interpretation**

The agent will interpret the required frequency response, gain, polarity, and implementation constraints. It will determine whether a low-pass or high-pass filter is appropriate and identify missing or conflicting requirements.

**2. Circuit topology selection**

The agent will select between predefined Sallen-Key and Multiple Feedback (MFB) circuit architectures.

The selection will consider the required gain and output polarity, component count, operational amplifier limitations, and expected circuit performance.

**3. Component design and optimization**

The agent will invoke analytical design tools and deterministic optimization methods to determine appropriate resistor and capacitor values.

Component selection will be restricted to the E12 series, initially using resistor values between 1 kΩ and 100 kΩ and capacitor values between 100 pF and 470 nF.

**4. Simulation-based evaluation**

The agent will execute candidate designs through a validated ngspice interface and analyze the resulting frequency-response measurements and operating conditions.

An independent verification layer will determine whether each design satisfies the specified engineering constraints.

**5. Failure diagnosis and adaptive redesign**

When a design fails, the agent will analyze the simulation results and diagnostic logs to identify possible causes.

It may adjust component-selection strategies, reconsider the circuit topology, investigate simulation failures, or identify specifications that cannot be satisfied within the available design space.

**6. Verification and reporting**

The agent will operate within predefined execution and design boundaries. It will not be permitted to execute arbitrary commands, modify unrestricted circuit netlists, or override mandatory engineering constraints.

The final design will undergo deterministic verification and human review.

The system will produce a structured report describing the selected circuit, component values, simulated performance, limiting constraints, and engineering decisions.

**Expected contribution of the LLM**

The LLM will coordinate interconnected decisions at different levels of the engineering workflow: interpreting requirements, selecting circuit architectures, choosing computational tools, diagnosing unexpected results, and adapting the design strategy.

Its effectiveness will be evaluated against conventional analytical design and deterministic optimization approaches using reproducible engineering test cases. The evaluation suite will also include negative controls and deliberately invalid designs to verify that the system correctly rejects circuits that violate engineering specifications, even when their simulations execute successfully. Evaluation metrics will include design success rate, computational efficiency, constraint compliance, and failure-diagnosis performance.

**Open question:**

How effectively can an LLM-based engineering agent coordinate filter-type selection, circuit topology selection, component optimization, and simulation-based diagnosis to produce valid analog filter designs under varying engineering constraints?

## Explain your project to someone outside your field

**My project in simple words:**

Electronic devices such as medical monitors, microphones, and industrial sensors measure electrical signals that carry information about the physical world, such as heartbeats, sound, or temperature. However, these measurements can contain unwanted electrical interference that makes the useful information harder to identify. Electronic filters help separate useful signals from unwanted ones by allowing certain frequencies to pass through while reducing others. A low-pass filter preserves slower changes while suppressing rapid fluctuations, whereas a high-pass filter preserves faster changes while suppressing very slow variations or background drift. Designing these circuits is challenging because engineers must choose the appropriate circuit structure, determine suitable electronic components, and account for the imperfect behavior of real amplifiers.

My project aims to create an AI-based engineering assistant that manages this design process. The assistant will receive a description of which frequencies should be preserved and which should be suppressed, determine the appropriate filter type, select a circuit architecture, and use engineering tools to calculate component values. It will then test the proposed circuit using a simulator that predicts its electrical behavior. If the circuit does not meet the requirements, the assistant will analyze the results and decide how to improve the design or whether further engineering intervention is necessary. The final result will be a simulation-verified circuit design accompanied by an explanation of its performance and the decisions made during the design process.
