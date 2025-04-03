---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.4
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

<div class="alert alert-block alert-info">

**Accessibility of the notebook:**
- You can adjust the width of the text with the function provided below. Copy it into a new code cell and execute it using the `ipykernel`.
    ```python
    from IPython.core.display import HTML
    def set_width(width):
        display(HTML(f"""<style>  
                .container {{ width:{width}% !important; 
                                min-width:800px !important; margin: 0 auto}} 
                .jp-Cell {{ width:{width}% !important; 
                                min-width:800px !important; margin: 0 auto}} </style>"""))
    # Set container width to X% of the fullscreen 
    set_width(50)
    ```
- You can toggle the auto-numbering of the sections in the outline toolbox (sidebar or topbar).
- You can toggle the code line numbers in the dropdown menu of the "view" button in the topbar. 
- You can collapse/expand a cell by clicking the blue bar on the left side of the cell.

</div>  

```python
from IPython.core.display import HTML
def set_width(width):
    display(HTML(f"""<style>  
            .container {{ width:{width}% !important; 
                            min-width:800px !important; margin: 0 auto}} 
            .jp-Cell {{ width:{width}% !important; 
                            min-width:800px !important; margin: 0 auto}} </style>"""))
# Set container width to X% of the fullscreen 
set_width(50)
```

Experiments of this week:
- experiment 14A: simulate a circuit to understand how differential mode is amplified and common mode is rejected
- experiment 14B: build a comparator (with hysteresis) circuit and explain its behaviour
- experiment 14C: build a magnetic sensor using instrumentation amplifer and understand how it works

Goal: Understand the use of differential mode, bridge sensors, a comparator (with positive feedback), and sensing a magnetic field. 

Structure of an experiment:
- Background+Anticipate + Simulate (50 min): per person. <br>
  <font color='red'>This is homework and should be finished **before** you start your 4 hours practicum session</font>
- Implement + Investigate (45 min): with your partner(group of 2)
- Compare + Conclude (10 min): with a group of 4(per table)


# 14A: Differential and common mode


## Goals

### Understanding Differential Signals

- **Context of Previous Circuits**: Previously, we've seen circuits with signals referenced to ground, where one side of the signal source is grounded and the other is connected to the circuit input. The output is similarly measured with respect to ground.
- **Differential Signals**: Often, we encounter signals not referenced to ground but to another potential in the circuit or system, known as differential signals. These are the differences in potential between two points (A and B).
- **Handling Differential Signals**: Despite not being ground-referenced, differential signals can be filtered, amplified, etc. Their average value (potential) isn't zero but is considered as a common offset, known as the common mode voltage.



## <font color='magenta'> This whole notebook uses only LTSpice. If you want, you can do it all at home before the practical session. </font>







## BACKGROUND
> <font color='grey'>⏳ Estimated time: 20 min</font>

## BACKGROUND
> ⏳ **Estimated Time**: 20 min

### Topics Covered:
- Differential and common mode signals
- Common Mode Rejection Ratio (CMMR)
- Wheatstone bridge

### Measuring Small Differential Signals
Suppose you need to measure a small differential (time-varying) signal that is overshadowed by a much larger common-mode signal. Previously, we've seen that an AC mode can be used for this purpose. But what if your goal is to amplify the small differential mode?

Using a single-ended amplifier (a standard amplifier type) would amplify both differential and common-mode signals equally. This approach makes it challenging to distinguish the small differential signal, as the amplifier is likely to saturate due to the high common-mode signal.

![Differential and Common Mode Signal](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_8.jpg)

### OPAMP Simulation: Differential Amplifier
In the OPAMP simulation session, focus will be on the differential amplifier, which involves an opamp and four resistors (two pairs with carefully chosen values). 

This setup is designed to measure **differential signals** = signals with the same amplitude but opposite phase (polarity).

The key advantage of a differential amplifier is its ability to amplify only the differential part of the signal while suppressing any common-mode signal present. This common-mode signal could be a large voltage offset or external noise, which are typically not of interest.

| Schematic of a Differential Amplifier | Voltage Reference Diagram |
| --- | --- |
| ![Differential Amplifier](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS9_1a.png) | ![Another Diagram](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_2.png) |

### Wheatstone Bridge: Differential and Common-Mode Signals
The Wheatstone bridge exemplifies a signal source with both differential and common-mode signals. Similar to the diode bridge, it consists of four resistive elements. Some of these resistors change their resistance in response to external factors like magnetic fields or temperature. The voltage between the terminals UR and UL represents a **differential** signal, while the half-bridge voltage is a **common-mode** signal (shared by both UR and UL).

#### Equations
- **Differential Equation**: 
  $$ U_{diff} = U_{R} - U_{L} = \frac{dR}{R} \times U_{b} $$

- **Common-mode Signal**: 
  $$ U_{com} = \frac{U_{R} + U_{L}}{2} = \frac{U_{b}}{2} $$
  $$ U_{L} = U_{com} - \frac{U_{d}}{2} $$
  $$ U_{R} = U_{com} + \frac{U_{d}}{2} $$


## ANTICIPATE: Gain of the Differential Amplifier
> ⏳ **Estimated Time**: 15 min

### Task: Calculate Differential Gain ($G_{d}$)
Calculate the differential gain ($G_{d}$) of the differential amplifier shown in the circuit below. Consider the following assumptions:
- Both voltage sources are ideal (output resistance = 0 Ω).
- They have the same common signal, with a small difference between the two signals (like 5V ± a pulse of 10mV).
- The OPAMP is assumed to be ideal.

![Differential Amplifier Circuit](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_8.jpg)

### Understanding CMRR
- **Explanation Required**: Describe what the Common Mode Rejection Ratio (CMRR) is.
- **Impact of Resistor Mismatch**: Discuss the effects on the CMRR if R1 is not exactly equal to R3, or if R2 is not precisely the same as R4. Explain the reasons behind these effects.


```python

```

```python

```

```python
### TO DO ="your prediction for ideal and when the resistor values almost (but not completely) match"

 
```

## SIMULATE: Making a Differential Signal
> ⏳ **Estimated Time**: 15 min

### Task 1: Generate a Differential Signal
The objective is to generate a differential signal (Vd+ and Vd-) superimposed on a common-mode signal Vcom. The setup involves:
- **Differential Signal**: A small pulse.
- **Common Mode Signal**: A large sinewave.

![Schematic for Differential Signal Generation](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_3.jpg)

#### Voltage Sources Configuration 
Enter the voltage sources description as indicated below. Note that the values for V3 are identical to V2.

| Voltage Source V1 | Voltage Sources V2 and V3 |
| --- | --- |
| ![Voltage Source V2 Description](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_4.jpg) | ![Voltage Source V3 Description](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_5.jpg) |

Note:
- **Inverted Signal**: Since V3 is connected with its + terminal to Vcom, its - terminal (Vd-) will exhibit an inverted signal shape compared to Vd+.

### Questions
1. **Total Amplitude of Differential Signal**: What is the total amplitude of the differential signal? Include a screenshot to support your finding.
2. **Visibility of Signal**: Can you clearly discern the differential signal just by observing Vd+ and Vd-?
## SIMULATE: Making a Differential Signal
> ⏳ **Estimated Time**: 15 min

### Task 1: Generate a Differential Signal
The objective is to generate a differential signal (Vd+ and Vd-) superimposed on a common-mode signal Vcom. The setup involves:
- **Differential Signal**: A small pulse.
- **Common Mode Signal**: A large sinewave.

![Schematic for Differential Signal Generation](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_3.jpg)

#### Voltage Sources Configuration 
Enter the voltage sources description as indicated below. Note that the values for V3 are identical to V2.

| Voltage Source V1 | Voltage Sources V2 and V3 |
| --- | --- |
| ![Voltage Source V2 Description](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_4.jpg) | ![Voltage Source V3 Description](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_5.jpg) |

Note:
- **Inverted Signal**: Since V3 is connected with its + terminal to Vcom, its - terminal (Vd-) will exhibit an inverted signal shape compared to Vd+.

### Questions
1. **Total Amplitude of Differential Signal**: What is the total amplitude of the differential signal? Include a screenshot to support your finding.
2. **Visibility of Signal**: Can you clearly discern the differential signal just by observing Vd+ and Vd-?



```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload

```

```python
### TO DO=" What do you observe from Vd+ and Vd-"
 
```

```python
file_name="14A_1_diffsignal.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

> **__<font color='blue'>Hints:</font>__**
> - Consider the non-zero difference between Vd+ and Vd-. 
> - Remember to account for the sign of each voltage.

> **__<font color='red'>Troubleshooting tip:</font>__**
> - If you're having difficulty discerning the difference between Vd+ and Vd-, try temporarily setting the sine wave to 0V and rerun the simulation for clarity.

### Task 2: Plotting the Differential Signal
1. **Plot Differential Signal**: Now, plot the differential signal *Vd+ - Vd-*. You should be familiar with how to plot an equation as a trace (right-click a trace in the plot of the simulation).
2. **Observations**: What do you notice about the differential signal and common mode?
3. **Evidence**: Upload a screenshot to support your observations and conclusions.
4. __<font color='red'>SAVE YOUR LTSPICE DRAFT. YOU WILL NEED IT DURING I&I. Otherwise, you will have to redo it</font>__

```python
### TO DO=" Do you see the common signal, differential or both signals. "
 

```

```python
upload
```

```python
file_name="14A_2_diffsignal_only.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

```python
#precap 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('UWEMglDP5Nk', width = 600, height = 450)
```

## IMPLEMENT & INVESTIGATE: <br> Simulate a Differential Amplifier Circuit
> ⏳ **Estimated Time**: 45 min

### Implement
The goal is to simulate a differential amplifier, an essential electronic circuit for measuring the difference between Vd+ and Vd-. This task expands upon the circuit used in the previous simulation.

![Differential Amplifier Circuit](https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS9_9.jpg)

#### Circuit Setup Instructions
- **Power Supply**: Note the dual power supply on the left, labeled as Vp and Vn, for powering the OPAMP.
- **Signal Connections**: Connect the signals Vd+ and Vd- to the inputs of the amplifier using labels. 
    - **Important**: Remember that the non-inverting input is below the inverting input.
- __<font color='orange'>Caution:</font>__ Ensure that the Vcom sinewave has a **1 volt** amplitude before simulating the circuit.

### Investigate
- **1. Output Signal Description**: After simulating the circuit, describe the output signal. Focus on two aspects:
  - Differential Signal Gain
  - Common-Mode Signal



> __<font color='blue'>Hint:</font>__
**You can reduce the font size** of the expressions appearing in the schematic, it occupies
quite some space. When you extend the schematic and need the space, rightclick the text to change font size.

```python
### TO DO=" how does the output depend on common mode and differential signal"
 
```

- **2. Identifying Common-Mode Sine-Wave:** Examine the output signal closely to identify the common-mode sine-wave. Here are the steps to follow:

    1. **Zooming In**: Focus on the y-axis scale of the output signal. Right-click on the y-scale and adjust it to zoom in, making the sine-wave visible.
    2. **Amplitude Measurement**: Determine the amplitude of the common-mode sine-wave once it becomes visible.

> __<font color='blue'>Hint:</font>__
> The common-mode signal is significantly suppressed (rejected), resulting in a very small amplitude. You may need to zoom into the microvolt (uV) scale to clearly observe the common mode signal.


```python
### TO DO=" What is the amplitude of the sine wave (zoom in!)"
 
```

- **3. Determine the gain ($G_{c}$) of the common-mode signal, as ratio and in dB.**

- **4. Determine the Common Mode Rejection Ratio (CMMR)**, the CMRR is defined as:
 $CMRR = \frac{G_{d}}{G_{c}} $

```python
### TO DO=" Derivation for CMRR"
 
```

#### Realistic Common-Mode Rejection Analysis
The common-mode rejection observed in this simulation is **unrealistically** high due to perfect conditions that are hard to replicate in real life. This is because:

- The resistors in the simulation are 100% matching.
- Both inputs of the OPAMP are identical, *which in practice is impossible to achieve.*

#### Adjusting for Realism
- **5. To simulate a more realistic scenario, make the following adjustment:**

    1. **Change Resistor Value**: Modify the value of resistor R3 to be 1% larger than its current value.
    2. **Re-evaluate Parameters**: After changing R3, re-determine the common-mode gain ($G_{c}$) and the Common Mode Rejection Ratio (CMMR).



```python
### TO DO=" write your answer here for the CMRR with R3+1%"
 

```

- **6. What is the drop in CMRR?**

```python
### TO DO="derive the change in CMRR due to mismatch in resistance values"
 
```

This is a more realistic value of CMRR, but in practice it is difficult to achieve such good preformance with this kind of circuit.


## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 10 min</font>

### Group Collaboration and Comparison
1. **Wait for All Members**: Ensure that all four group members have completed their observations.
2. **Compare Results**: Discuss and compare your findings with the rest of your group members.
3. **Consensus and Verification**:
   - If your results are consistent and align with predictions, call a TA for verification and check-off.
   - If there are discrepancies in your results, or they don't match your predictions, discuss these among your group before seeking assistance from a TA.

### TA Check-off Requirements
1. What was the ideal CMRR and the more realistic CMRR?
2. How do you calculate ($G_{d}$) ?
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved

```python
#14A differential and common mode
### TO DO="1. ideal CMRR"

### TO DO=" 2. how do you calculate Gd?"
 
### TO DO="3a. abstract"
 
### TO DO="3b. troubleshooting"
 
### TO DO="3c. code"
 
### TO DO="4. what changes would you suggest?"
 


```

```python
#recording 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('mrvtgzx2Cik', width = 600, height = 450)
```
