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

# 14B: Comparator 

> <font color='blue'>Learning goal:</font> Experiments with Differential and Common Mode Voltages

 - **Focus of Experiments**: We'll explore how to suppress unwanted common mode voltage and amplify the desired differential voltage.
 - **Practical Sensor-Bridge Application**: The experiments will include using a sensor-bridge sensitive to magnetic fields. This involves detecting the magnetic field created by current passing through a wire or a printed circuit board track.
  
## Structure of the practical assignment 14B:
- Background + Predict + Simulate (45): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Build + Measure (40): with your partner(group of 2)
- Evaluate (10): with a group of 4(per table)


## BACKGROUND
> <font color='grey'>⏳ Estimated time: 15 min</font>

### 1. Exploring OPAMP as a Comparator
In this segment, we delve into the use of an Operational Amplifier (OPAMP) in a unique role—as a comparator. This setup is intriguing because it leverages the OPAMP's high gain without requiring negative feedback. Essentially, the OPAMP's output oscillates between two extremes, clipping against either its positive or negative supply rail, indicating a binary state: either fully positive or fully negative.

### 2. Comparators vs OPAMPs
It's important to highlight that there are specialized Integrated Circuits (ICs) designed as comparators. While bearing a resemblance to OPAMPs with differential inputs and high gain, comparators are distinctly engineered to output just two levels: high or low. Unlike OPAMPs, they are unsuitable for linear amplification tasks.

### 3. Comparator Circuit Analysis
The comparator circuit employs one input for reference and another for the signal. When the reference voltage (Vd) is connected to the inverting input and the signal voltage (Vi) to the non-inverting input, the circuit operates in the following manner: If Vi is less than Vd, the output is negative (since Vi - Vd yields a negative result). Conversely, if Vi exceeds Vd, the output turns positive.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14B_comparator.png" width="100%">
</div>

Above is the circuit diagram and its typical behavior.

> What would change if we would swap both inputs? So Vi to the inverting input and the reference (Vd) to the non-inverting input?

### 4. Dealing with Noisy Signals
In an ideal world, signals are noise-free, but reality presents us with noisy signals that can lead to erratic behavior in the circuit, as shown below.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14B_noisy_signal.png" width="70%">
</div>

Noise-induced fluctuations, or 'bouncing', can be problematic. It depends on the applications, but especially in scenarios where the comparator feeds into a counter, "bouncing" will lead to counting errors. To mitigate this, we introduce hysteresis into the circuit, altering the switching threshold based on the current output state.

Changing of the switching levels, the hysteresis, is achieved via positive feedback where the output is connected back to the **non-inverting input** through a set of resistors. 

> Note: this is not a linear circuit! So we cannot assume that inverting and non-inverting input have the same potential. However, we still assume that no current is flowing into the OPAMPs inputs.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14B_non-inverting_comparator.png" width="600">
</div>

The diagram above showcases **a non-inverting comparator.** The output is positive when the input signal surpasses the threshold and negative otherwise.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14B_switching_level.png" width="600">
</div>

#### 5. Calculating the Switching Levels
To determine the switching levels, recognize that:
1. The OPAMP compares the voltage on the non-inverting input to the potential on the inverting input (GND).
2. The potential on the inverting input is stems from the resistive divider R1 and R2, which are connected between the input voltage and the output voltage.
3. At the moment of switching, the potential on the non-inverting input is exactly 0V, meaning that the voltage drop over R2 is equal to Uout, and so the current is Uout/R2.
4. The current through R2 flows through R1, creating a voltage drop over R1: \$(U_{out}/R2) * R1\$
5. Thus, the switching levels are calculated as ± Uout * (R1/R2).


<!-- #region -->
## ANTICIPATE: Switching Levels in a Comparator with Hysteresis
> <font color='grey'>⏳ Estimated Time: 15 min</font>

### Understanding the Schmitt-Trigger Comparator
This section focuses on calculating the switching levels of a comparator circuit with hysteresis, commonly known as a Schmitt-trigger. The circuit diagram below illustrates the comparator in question. For this calculation, assume the following:
- The positive output voltage of the OPAMP is +12V.
- The negative output voltage of the OPAMP is -12V.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC13_1.jpg" width="50%">
        <br>
    <em>Figure: Comparator with Hysteresis, featuring resistances Ra = 10kΩ and Rc = 3.4kΩ.</em>
</div>


Your task is to determine the specific points at which this comparator switches its output state, considering the given resistor values and the OPAMP's output voltage limits. This exercise is crucial in understanding how hysteresis impacts the behavior of comparators in real-world applications.

<!-- #endregion -->

```python
### TO DO="write your answer for comparator switching-levels " 

```

## SIMULATE: Building and Analyzing a Comparator without Hysteresis
> <font color='grey'>⏳ Estimated Time: 15 min</font>

### Constructing the Comparator Circuit
Your task in this section is to construct a comparator circuit using an LM741 operational amplifier (opamp). To ensure accurate simulation results, remember to power the LM741 with a ±12V supply. The circuit diagram to guide your construction is provided below:

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC13_2.jpg" width="50%">
</div>

### Simulation
Once your circuit is built, proceed with a simulation to observe the voltage changes over time. 
- For the simulation, set your input voltage source to have a 1V amplitude and a frequency of 100Hz. 
- Pay special attention to the voltage across the components labeled as U16 and U6 in your circuit.

### Questions
- What changes in voltage do you observe in the simulation?
- How does the output voltage respond to the input signal?

> **<font color = 'blue'> Hint: </font>**
Look closely where the "switching" happens and what are the max values

Upload screenshot of your simulation results showing the voltage changes over time

```python
### TO DO=" If you input a sine, explain how will the output look like (shape, phase, amplitude) "
 
```

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python
file_name="14B_1_comparator_sim.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

Now switch the inputs of opamps' inputs. Looking at the graphic, you should connect U16 to U5 and U4 to U101-GND. <br>
What changed, compared to the previous exercise?


### Modify the Comparator Configuration
In this next step, you'll modify the comparator circuit by switching its inputs. Refer to the original circuit diagram and make the following changes:
- Connect U16 to U5.
- Connect U4 to U101-GND.

Once you have rearranged the inputs as described, run the simulation again with the same settings as before (1V amplitude, 100Hz frequency). Observe the changes in the comparator's behavior and output.

**Question:** What differences do you notice in the comparator's output compared to the previous configuration?

```python
### TO DO=" write your answer: the difference is ..."
 
```

```python
#precap  
from IPython.lib.display import YouTubeVideo
YouTubeVideo('kaO_t4k1ggw', width = 600, height = 450)
```

<!-- #region -->
## IMPLEMENT & INVESTIGATE 1: Building a Comparator Circuit with Hysteresis
> <font color='grey'>⏳ Estimated Time: 40 min</font>

### Equipment Required
To construct and investigate the comparator circuit, gather the following items:
- Testboard 4
- Function generator
- Oscilloscope
- Coaxial splitter
- All necessary cables

### Circuit
Build a comparator circuit with hysteresis on Testboard 4. Refer to the circuit diagram provided below for guidance:

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC13_3.jpg" width="800">
    <br>
    <em>Figure 3: Comparator with Hysteresis.</em>
</div>


### Setting Up the Experiment
1. Set the resistances as follows:
    - Ra to 10kΩ (activate switch #3).
    - Rc to 3.4kΩ (turn on switches #2 and #3, paralleling 10kΩ and 5.2kΩ resistors).
2. Connect the signal generator to the **input** of the comparator circuit.
3. Use the coaxial splitter to connect **CH1** of the oscilloscope to the signal generator.
4. Connect the **output** of the comparator circuit to **CH2** of the oscilloscope

### Configuration Parameters 
Configure the signal generator with these settings:
- Waveform: Sine
- Output Impedance: High Z
- Amplitude: 17V peak-to-peak (Vpp)
- Offset: 0V
- Symmetry: 50%
- Frequency: 200Hz

### Questions
Monitor the input and the output of the circuit simultaneously. 
1. Did you expect this behaviour? 
2. How does it compare to results from SIMULATE?

<!-- #endregion -->

> **<font color = 'blue'> Hint: </font>**
You should see a shift in your signal due to these modifications. What parameter was changed by changing the resistor values (think about the anticipate and simulate).

```python
### TO DO=" write your answer"
 
```

### Analyzing Switching Levels Using Oscilloscope's XY-Mode

After setting up your comparator circuit, it's time to analyze its switching levels in a more visual and insightful way:

1. **Switch the oscilloscope to XY-Mode**: 
   - Locate and press the “main/delayed” button on your oscilloscope.
   - Change the display mode from Y-T (normal) to X-Y.
   
2. **Investigate and document switching levels**: 
   1. Examine the oscilloscope screen to determine the comparator's switching levels.
   2. Make an accurate drawing of the figure displayed on the oscilloscope screen. 
       - Ensure that your drawing captures the key features and thresholds of the waveform.
       - Alternatively, you can take a clear photograph of the oscilloscope screen.

   


```python
### TO DO=" your answer, supported by an image of the scope screen" 
 
```

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python
file_name="14B_2_switching_levels.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

<!-- #region -->
### Studying Low-Speed Comparator Behavior

Now that you have observed the comparator's switching levels, the next step involves analyzing its behavior at a significantly lower frequency:

1. Set the frequency of the signal generator to 0.1 Hz. This change will cause the curve on the oscilloscope to be traced at a much slower pace.

2. Carefully watch the curve as it slowly follows the input signal. 

    - Pay attention to how the comparator reacts at this reduced speed. 
    - Try to understand and explain the changes you observe in the circuit's behavior. <br>

**Question:** How does the comparator respond to the slower input signal transitions?


> **<font color='blue'>Hint:</font>** If you are having difficulty observing the curve due to its slow movement, consider adjusting the persistence setting of the oscilloscope screen to 'infinite'. This setting will make the trace remain visible longer, aiding in your analysis.

> **<font color = 'blue'> Hint 2: </font>**
A comparator circuit has the output reacting to the input. Why would changing the frequency influences the way this happens?
<!-- #endregion -->

```python
### TO DO=" your answer, what do you see and how can you explain" 
 

```

## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:**

1.  What are the advantages of using hysteresis in comparator circuits.
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved


```python
#14B comperator
### TO DO="1. the advantage of hysteresis"
 
### TO DO="2a. abstract"
 
### TO DO="2b. troubleshooting"
 
### TO DO="2c. code"
 
### TO DO="3. what changes would you suggest?"
 
```

```python
#recording 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('RBpHN07FWME', width = 600, height = 450)
```
