# SweetSpot Glucometer Project

## About

SweetSpot is a computer-dependent electrochemical glucose sensing prototype designed to evaluate whether a commercial glucose test strip, paired with an IO Rodeostat potentiostat, can be used to measure glucose concentration through chronoamperometry.

The long-term goal of this project is to develop a low-cost and accessible glucometer system that can measure glucose concentration, process the signal, and display the result in real time. The project is being developed in stages, beginning with a laptop-based prototype for sensing validation and eventually progressing toward a standalone embedded device.

At its current stage, the system successfully demonstrates electrochemical signal acquisition, data processing, and calibration framework development. However, the sensing model has not yet been validated for reliable glucose prediction across repeated measurements.

---

## Project Goals

The project was designed to build a system that can:

- run a chronoamperometry measurement on a commercial glucose test strip
- extract a meaningful electrochemical feature from the current response
- convert that feature into glucose concentration using an experimentally derived calibration curve
- display the result digitally in real time

The development plan includes two major stages:

1. **Computer-dependent prototype** for validating sensing and calibration  
2. **Standalone embedded prototype** for integrated measurement and display

---

## Why This Project Matters

A glucometer is a portable medical device used to measure blood glucose concentration. It is essential for diabetes monitoring and management, especially for individuals who need frequent readings to guide daily decisions.

As diabetes prevalence continues to rise, the need for affordable and accessible glucose monitoring tools becomes more urgent. This challenge is particularly important in underserved urban communities, where access to regular testing and early diagnosis may be limited. Our project is motivated by the idea that a lower-cost electrochemical sensing platform could eventually support broader glucose awareness and improve access to screening.

---

## Scientific and Engineering Basis

Electrochemical glucometers work by coupling a selective biochemical reaction to an electrical measurement.

Commercial glucose test strips typically rely on **glucose oxidase**, which reacts selectively with glucose in the sample. This reaction produces an electroactive species that can be detected at the electrode surface. When the potentiostat applies a fixed potential, oxidation at the working electrode generates a measurable current. That current is then used as the sensing signal.

This project uses **chronoamperometry**, where the potential is stepped to a fixed value and the current is measured over time. Under ideal diffusion-controlled conditions, the expected behavior is described by the **Cottrell equation**, which predicts that current decreases with the inverse square root of time and is proportional to analyte concentration at a fixed time point.

Because of this relationship, features such as:

- maximum current
- current at a fixed time
- current decay behavior

can, in principle, be used to estimate glucose concentration.

This theoretical framework guided our calibration strategy and feature selection.

---

## Current Project Status

At this stage, the project functions as a **computer-dependent electrochemical sensing prototype**.

### What is working

- electrochemical signal acquisition with the IO Rodeostat
- Python-based data collection in Jupyter Notebook
- current-versus-time visualization
- export of raw data to CSV
- construction of calibration relationships from experimental data

### What is not yet working

- reliable glucose concentration prediction
- validated sensing model across repeated measurements
- Arduino communication
- OLED display output
- standalone device operation

Although the system can successfully run chronoamperometry and collect current data, the measured signal does not yet translate into sufficiently accurate and reproducible glucose predictions.

---

## Progress Log

## February 12, 2026

We connected the IO Rodeostat potentiostat to a laptop and performed an initial chronoamperometry test using a commercial glucose test strip and legacy connector leads.

The purpose of this first experiment was to confirm that the Rodeostat could apply a potential step and record a measurable transient current response over time. At this stage, the goal was not yet accurate glucose prediction. Instead, the focus was on verifying that the hardware and software could produce a usable electrochemical signal.

During this phase, we developed a Python script in Jupyter Notebook to interface with the Rodeostat, capture the current response, and generate current-versus-time plots. This established the software foundation for later calibration, visualization, and signal analysis.

From an engineering standpoint, this phase served as a feasibility test of the acquisition system. Before quantitative sensing can be evaluated, the instrumentation must first demonstrate that it can consistently produce and record a usable signal.

## February 26, 2026

We prepared a full set of standard glucose solutions for calibration and improved the data pipeline used to process measurements.

The Python workflow was updated to:

- generate current-versus-time plots
- export raw experimental data to CSV files
- improve reproducibility and recordkeeping for later analysis

At this stage, we collected the first full datasets for high and very high glucose concentrations. This marked the transition from basic signal verification to quantitative sensor characterization.

The electrode configuration used during testing was:

- **working electrode** = gray
- **reference electrode** = white
- **counter electrode** = black

Maintaining a consistent electrode configuration was essential for preserving the validity of the measurements.

## March 4, 2026

We completed the initial concentration measurements across all prepared glucose standards.

This was an important milestone because it provided the first full dataset for comparing electrochemical responses across concentrations. With these data in hand, the project shifted from signal collection to calibration analysis.

At this point, we began evaluating whether the measured current contained sufficient and reproducible information to distinguish among glucose concentrations in a physically meaningful way.

## March 5, 2026 — Initial Calibration Curve

Using the initial dataset, we constructed our first calibration relationship between measured current and glucose concentration:

**Glucose concentration (mM) = 1.1978 × (max current in μA) − 0.4567**

This equation represented the first working mapping from sensor output to predicted glucose concentration. In principle, once the maximum current from a run was measured, the calibration equation could be used to estimate glucose concentration.

The use of current as the sensing variable was grounded in chronoamperometric theory. Under ideal diffusion-controlled behavior, the Cottrell equation predicts that current should scale with concentration. Because of this, features derived from the current response, such as maximum current or current at a specified time, were reasonable candidates for calibration.

At first, this relationship appeared promising. However, later validation showed that the calibration did not remain reliable across repeated measurements or across the full concentration range.

---

## Key Findings

## Breakdown at High Concentrations

We observed that the calibration relationship becomes unreliable at very high glucose concentrations, especially in extreme diabetic ranges. In those cases, the response becomes nonlinear, and current no longer scales predictably with concentration.

This behavior is consistent with the limitations of commercial glucose test strips, which are generally designed to operate within typical physiological glucose ranges rather than extreme concentrations.

Possible causes include:

- enzyme saturation within the strip chemistry
- mass transport limitations near the electrode surface
- deviation from ideal diffusion-controlled behavior
- nonlinear reaction kinetics

From a chemical engineering perspective, this is a classic example of a system leaving its ideal operating regime. Once the assumptions behind the model no longer hold, the calibration relationship breaks down.

## Validation Testing

To determine whether the system could reliably predict glucose concentration, we performed repeated validation testing and compared multiple feature extraction methods.

We evaluated:

- **peak current**
- **steady-state or fixed-time current**

Although calibration curves could be generated for both methods, neither one produced predictions that were sufficiently accurate or reproducible across new trials.

This result suggests that the issue is not limited to one particular feature extraction method. Instead, it indicates a broader limitation in the sensing system itself.

---

## Engineering Interpretation of Failure

The fact that both feature extraction approaches failed suggests that the system is not behaving according to the idealized diffusion-controlled model assumed by the Cottrell equation.

Several possible explanations are being considered:

### 1. Strip–Instrumentation Mismatch

Commercial glucose test strips are designed to work with proprietary timing windows, voltage protocols, and internal electronics. The simple chronoamperometric step applied by the Rodeostat may not match the intended strip operating conditions.

### 2. Mass Transport Limitations

The Cottrell equation assumes a predictable diffusion layer and well-defined transport behavior. If glucose transport is inconsistent or affected by strip geometry, convection, or internal structure, the signal may vary independently of concentration.

### 3. Nonlinear Reaction Kinetics

The strip chemistry may introduce nonlinearities through enzyme kinetics, intermediates, or saturation effects. If the reaction is not first-order with respect to glucose, the current will not scale linearly with concentration.

### 4. Signal Instability

Small variations in strip condition, contact quality, or timing may strongly affect the measured current. Since both calibration methods depend on signal consistency, this variability reduces predictive reliability.

---

## Hardware Verification

We also considered whether the Rodeostat itself might be the source of the problem.

To investigate this, we:

- confirmed the device setup
- checked wiring and connections
- inspected the Rodeostat for obvious faults

No clear hardware issue was identified. This supports the conclusion that the main limitation is not a fundamental device failure, but rather incompatibility between the sensing method and the strip measurement conditions.

---

## System Architecture

Even in its current state, the project follows a modular sensing pipeline:

### Sensor Layer
Commercial glucose test strip produces current through an electrochemical reaction.

### Acquisition Layer
IO Rodeostat applies the voltage protocol and records current over time.

### Processing Layer
Python in Jupyter Notebook performs:

- data collection
- signal visualization
- feature extraction
- calibration analysis

### Output Layer (Current)
Laptop-based plots and predicted glucose values.

### Output Layer (Planned)
Arduino plus OLED display for real-time standalone readout.

---

## Prototype Plan

## Prototype 1: Computer-Dependent System

In the first prototype, the laptop serves as the main control and processing platform. The Rodeostat performs the measurement, Python processes the signal, and the eventual plan is for the laptop to send the calculated glucose value to an Arduino for display.

### Purpose

- validate electrochemical sensing performance
- simplify data processing during early development
- reduce embedded system complexity
- support rapid iteration and debugging

## Prototype 2: Fully Standalone Embedded System

The second prototype would move all major functions to embedded hardware. In that version, the microcontroller would handle signal acquisition, feature extraction, glucose calculation, and OLED display directly.

### Purpose

- demonstrate full hardware and software integration
- eliminate dependence on external computing
- create a portable and self-contained platform
- establish a path toward future miniaturization

At present, progression to Prototype 2 is being deferred until the sensing model is validated.

---

## Preliminary Calculations

### Blood Simulant

To approximate the electrolyte content of blood, a 100 mL NaCl stock solution at 140 mmol/L was prepared.

\[
(140 \text{ mmol/L}) \left(\frac{1 \text{ mol}}{1000 \text{ mmol}}\right)\left(\frac{58.44 \text{ g}}{1 \text{ mol}}\right)\left(\frac{100 \text{ mL}}{1000 \text{ mL}}\right)=0.818 \text{ g NaCl}
\]

### Glucose Standards

Glucose standards were prepared by adding known masses of glucose to 25 mL of stock solution. These standards were used to generate calibration data and evaluate how the electrochemical response changed with concentration.

---

## Market Relevance and Application

The long-term target market for this device is underserved and low-income populations, particularly in urban environments where diabetes prevalence is high and access to healthcare may be limited.

Potential user groups include:

- adults at elevated risk for Type 2 diabetes
- low-income individuals and families
- undiagnosed or pre-diabetic individuals
- community clinics and nonprofit screening programs
- students, uninsured individuals, and older adults who need simpler low-cost tools

The larger design goal is not to create a premium consumer medical device, but rather to explore the feasibility of a low-cost sensing system that could support broader glucose awareness and access.

---

## SWOT Analysis

### Strengths

- strong medical relevance and recurring demand
- real-time signal acquisition and feedback
- portable sensing concept
- user-oriented design potential
- strong interdisciplinary combination of electrochemistry, instrumentation, and data analysis

### Weaknesses

- sensing accuracy is not yet validated
- current calibration relationships are not reproducible enough
- dependence on laptop-based processing
- hardware integration remains incomplete
- small-scale development limits practical deployment

### Opportunities

- rising diabetes prevalence increases need for affordable monitoring tools
- future optimization could improve manufacturability and reduce cost
- embedded electronics and biosensor advances create pathways for refinement
- strong potential for educational, screening, and community-health applications

### Threats

- competition from established commercial glucometers
- regulatory barriers for medical sensing devices
- liability concerns associated with inaccurate glucose prediction
- possible incompatibility between commercial strips and non-proprietary instrumentation

---

## Future Work

Our immediate focus is to resolve the sensing limitation before proceeding with display integration.

### Next steps

- test alternative voltage protocols that better match strip operation
- explore additional signal features such as integrated current or decay fitting
- improve signal processing through smoothing and baseline correction
- investigate strip-specific operating requirements
- evaluate whether commercial strips are fundamentally compatible with Rodeostat-based sensing

Once a reliable sensing model is established, the next phase will include:

- Python-to-Arduino serial communication
- OLED display integration
- transition toward standalone embedded operation

---

## Final Project Definition

The current system is best described as a:

**computer-dependent electrochemical sensing prototype with unresolved measurement accuracy limitations**

It successfully demonstrates:

- signal acquisition
- data processing
- calibration construction

It does not yet demonstrate:

- reliable quantitative glucose sensing
- validated concentration prediction
- standalone real-time hardware display

That makes the current project a successful sensing-platform prototype, but not yet a validated glucometer.

---

## Current Repository / Report Contents

- project background and motivation
- scientific basis of electrochemical glucose sensing
- progress log and calibration development
- failure analysis and engineering interpretation
- prototype roadmap
- preliminary market and SWOT analysis
- next-step development plan
