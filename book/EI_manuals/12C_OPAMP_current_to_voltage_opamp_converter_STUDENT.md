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
    display_name: Alpaca Kernel
    language: python
    name: alpaca
---

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

# 12C: current-to-voltage converter
> <font color='blue'>Learning goals:</font> Understand why would we use current-to-voltage opamp-based converter instead of a resistor-based converter. <br>
Structure of this experiment:
- Predict + Stimulate (15+15+20 min): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Build + Measure (75 min): with your partner (group of 2)
- Evaluate (20 min): with a group of 4 (per table)



## BACKGROUND
> <font color='grey'>⏳ Estimated time: 15 min</font>

### Current-to-voltage converter from an inverting opamp

In a properly functioning linear circuit based on an operational amplifier (OPAMP), the 'zero conditions' are met. Refer to the diagram below, which illustrates a typical inverting amplifier configuration.
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/opamp12C_1.png" width="700px"/>
</div>

- Applying the zero-condition principle, we understand that there's no voltage difference between the inputs of the OPAMP. Consequently, the potential at the inverting input is 0 V.

- This creates a point at 0 Volts, making it appear similar to a grounded point. However, this point is unique in that the current flowing to it doesn't dissipate into the circuit's ground system. Instead, it continues through the resistor Rf. <br>

- The OPAMP maintains this potential (0 V) by adjusting its output. 

- **Therefore, the output voltage provides an indication of the current flowing into this virtual ground point.**

Consider an ideal current meter, which exhibits neither resistance nor voltage loss. In this context, our circuit appears ideal for monitoring a current flowing to ground. 

 - By setting the value of Ri to zero (replacing it with a wire), we establish a current measurement point at a constant ground potential. 
 - The voltage output at the OPAMP, influenced by the known transfer ratio determined by Rf, converts current to voltage. 
 - This output can then be connected to an Analog-to-Digital Converter (ADC) for further analysis.

### Photodiode

This circuit is particularly effective for measuring the current produced by a photodiode. A photodiode is a unique semiconductor device that generates a photocurrent when exposed to light. This photocurrent is directly proportional to the intensity of the incident light. When used in different configurations, particularly with an op-amp, the photodiode exhibits distinct characteristics that can be regarded as a light-intensity controlled current source or a voltage source.

These two modes that a photodiode can be operated in are explained below: 
    
1. **Photoconductive Mode**: 

    - In this mode, the photodiode is grounded without any resistor on the input branch, enhancing its responsiveness to light. When light strikes the photodiode, it generates a photocurrent, otherwise knowns as: reverse-current (or leakage current). 
    - By coupling the photodiode via a low-ohmic impedance (just a simple wire) to the inverting (-) input of the opamp, we can convert this current into a measurable voltage output. Below, the diagram shows how to connect a photodiode as a current source:
        <div>
        <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/opamp12C_PC.png" width="700px"/>
        </div>
    - When we connect the photodiode to a **current to voltage converter**, the diode will be practically shorted between the inputs of the OPAMP. (Note: the feedback circuit at the OPAMP, will create the low impedance, not the inputs of the OPAMP themselves!)
    - <i>The current-mode can be promoted further by connecting it in such a way that it is reversed biased, but in this experiment, we will not look into reverse biasing.</i>
    - This mode emphasizes the photodiode's role as a current source, where **the output voltage is linearly related to the light intensity and the feedback resistor $V_{out} \propto I_{light} \cdot R_{f}$,  making it preferable for precise measurements.**
    - This means a photodiode generates;
        - a very low current when illuminated with with low intensity light and 
        - a very high current when illuminated with high intensity light. 
    - One could say that diode is less sensitive, however its output signal is very suitable for intensity measurements in bio-chemical experiments because of its excellent linearity. 
    - In measurement applications the photodiode will mostly be used in photoconductive mode (current-mode).
<br>   

2. **Photovoltaic Mode**: 

    - Alternatively, in the photovoltaic mode, the photodiode operates without any external bias (zero-bias) or also grounded, but with a high load resistor on the input branch leading to a high input impedance. This operation is similar to a solar cell. Here, it generates a voltage in response to light exposure. This mode of operation can be seen as: **voltage amplifier (inverting or non-inverting)**
    - By again coupling the photodiode to the inverting (-) input of the opamp, this voltage can be amplified and observed as a output voltage. 
        <div>
        <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/opamp12C_PV.png" width="700px"/>
        </div>
    - In this configuration, the photodiode acts as a voltage source (the current is converted to voltage on the resistor), and the output voltage varies with the intensity of the incident light. The voltage generated by the photodiode is again proportional to the light intensity, but the relationship may not be perfectly linear. 
    - **The voltage-intensity curve in this case would generally start linear but could show a leveling off (decreasing sensitivity and eventually saturation) at higher light intensities, which is reminiscent of a switch-like behavior or a very sensitive but an inaccurate detector**  

<!-- #region -->
## ANTICIPATE : compare resitor based current-to-voltage converter to opamp based one
> <font color='grey'>⏳ Estimated time: 15 min</font>

In BACKGROUND section, you have seen two setups of the photodiode. 
1. In one, opamp converted current to voltage, 
2. While in the other one, the resistor did it. 

In this section we explore whether it is better to use resistors only, or also an opamp, to read out a diode. For this we use the following 2 circuits, with a load resistor:

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/A2-12C.png" width="700px"/>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/A1-12C.png" width="700px"/> 
        
    

**Make some predictions:**<br> 

1. Do you think the current flows through the opamp?
<!-- #endregion -->

```python
### TO DO ='Explain whether current flows through the opamp' 

```

2. What is the overall impedance in the *resistor circuit* (with Ri and Rload)? 
3. What do you think is a drawback? *Hint: think how change in load affects the output (Vout).*

```python
### TO DO ='the impedance of the resistor circuit' 

```

You already know the Vout of the *opamp circuit*.

4. Can you show how it is derived (type out the derivation)?

```python
### TO DO ='your derivation for Vout' 

```

You might have noticed that the load does not affect the formula for the *opamp circuit* while it does for *resistor circuit*.

5. Based on that (and information from the BACKGROUND), can you say which setup would you prefer to use?

```python
### TO DO ='your prefered setup plus explanatoin why' 

```

> <font size=4>ℹ️</font> <b>Hints</b>
You can learn more about opamp circuit with load attached on this website : 
https://www.allaboutcircuits.com/video-tutorials/op-amp-applications-current-to-voltage-converter/


## SIMULATE: 
> <font color='grey'>⏳ Estimated time: 20 min</font>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/sim_setup.png" width="700px"/>

Build the above circuit of the inverting amplifier with the following exercises, and verify whether your predictions were correct.

**1**: The current source is an (AC) current source. Set the current to 1uA amplitude and a frequency of 1 kHz.

**2**: Run the simulation. Write down the maximum output voltage and the voltage at the inverting input in one graph. 

```python
### TO DO =' write your answer here, what is Vout for 1uA + explain' 

```

**3**: 
1. Increase the amplitude of the AC-current to 1mA, 
2. Look at the output voltage and inverting input again.
3. Then increase the current to an amplitude of 2mA. 


```python
### TO DO =' write your answer here, what is Vout for 1mA, 2mA + explain' 

```

**4**: Can you explain what is happening?
> __<font color='blue'>Hint:</font>__
What voltage do you expect from this point? Can you get such high voltage out?

<br>
Use the code below to insert your image of the 2mA simulation(screendump of LTSpice simulation).

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python
file_name="12C_ItoV_higherI.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

As you can see, the zero-conditions do not hold anymore in the last case.<br>

**5**: Can you explain what is the drawback of using *opamp circuit* for current to voltage conversion in this case? 

**6**: Would *resistor circuit* face similar challenges?


```python
### TO DO ='your answer what the drawback is of the opamp circuit, and explanation whether the resistor circuit would face similar challenges' 

```

<!-- #region -->
## IMPLEMENT & INVESTIGATE: <br>Photodiode in the Photoconductive and Photovoltaic modes 
> <font color='grey'>⏳ Estimated time: 75 min</font>

> during this exercise you will see colored blocks : blue and yellow. Blue blocks provide hints/tips while yellow blocks provide warnings

### Introduction

In this section, we will build an experimental setup to observe the different modes of operation of a photodiode.  We will later measure the $V_{output}$ of the OPAMP as a function of the light intensity , $I_{LED}$, shining on the photodiode, so effectively we will acquire curves that approximately capture the essence of $V_{output}(I_{LED})$

<div class="alert alert-block alert-info">    
You may assume that the intensity of the light emitted from LED is a linear function of the voltage applied to the LED.
    
- Keep in mind however that you must first overcome the <i>forward voltage</i> of the LED. 
- For simplicity, we will perform a simple voltage sweep using the DAC's function generator and set it to `Triangle`wave from $V_{min} = 0\text{V}$ to $V_{max} = 3.3\text{V}$ with a rather low frequency of $f_{LED} = 0.095\text{Hz}$ for only a quarter of the period to mimic a positive voltage sweep.
</div>

### Part A: Build the experimental setup



It turns out that this measurement requires some creativity for the design and some caution to protect your device, so this setup is quite complicated. Whenever you're facing a challenging setup, it's a good idea to break it down into smaller parts. In this case, you will need three essential components:

1. Alpaca's main breadboard for the OPAMP circuit
2. Photodiode illumination setup
3. Alpaca's attenuating opamps for protecting Cria's ADCs from excessive input voltage during your measurement

<!-- #endregion -->

#### 1. OPAMP Circuit
Use Alpaca's main breadboard to make the usual connections for an opamp in the inverting amplifier setup
1. Use the TL072IP opamp
    <div>
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/7b873596c01a500f66c42c89508ee5aa384b6335/voltammetry/opamp_dual_layout+component.jpg" width="300px"/>
    </div>
2. Use $100k\Omega$ as the feedback resistor
3. Remember to supply $\pm 12\text{V}$, but **keep it switched off whenever there's a risk of damaging the opamp**
4. Connect the opamp's non-inverting input to GND
5. Plug in the the wires for the inverting input and the output of the opamp into the breadboard, but keep the other ends disconnected for now. 

<div class="alert alert-block alert-info">
    <details>
  <summary> <font size=4>ℹ️</font> <b>Hints</b>: Detailed schematics of the OPAMP circuit</summary>  
    Drawing (Note that the wire from the inverting input is still missing here)
 <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_opamp.png" width="400px"/>
</details>
</div>    


#### 2. Photodiode illumination setup
Use the mini-breadboard to build a photodiode illumination setup. 
<details>
<summary><font size=4>ℹ️</font> <b>Detailed instructions</b></summary>

1. Place the photodiode and a white LED on the opposite ends of the mini-breadboard. <br>
    Also, to easily remember the polarity of your diodes, 
    - plug in the long legs into the 3rd pin away from the center
    - and the short leg into the 2nd pin on the other side counting from the center axis of the box.
2. Add a $100\Omega$ resistor to limit the current through the LED. 
    - Place one end in the line with the cathode side of the LED, short leg (-), 
    - and using a wire, connect its other end with GND on Alpaca. 
3. Plug in a wire on the anode side of the LED, long leg (+) and connect the other end to the DAC A output on the Alpaca.
4. Connect the anode, long leg (+) of the photodiode to the GND on Alpaca.
5. Add two $22\text{k}\Omega$ resistors combined in parallel or (almost) equivalently a single $10\text{k}\Omega$ resistor
    - Place one end(s) in the line with the cathode side of the Photodiode, short leg (-),
    - Place the other end(s) a few lines away, somewhere in the middle of the board (on the same side)
    - **Notice that you can change the mode of operation of the photodiode by including/omitting this load resitor from your circuit. It is enough to move the free end of the wire from the inverting input of the opamp between these two mentioned lines.**
6. Finally, adjust the position of the LED and the photodiode to face each other. 
    - We will use a piece of a drink pipe to keep the lenses in place and minimise the effects of the ambient light on the measurement.

</details>

<div class="alert alert-block alert-info">
    <details>
  <summary> <font size=4>ℹ️</font> <b>Hints</b> Detailed schematics of the illumination box:</summary>
      (Note that the wire to the inverting input is still missing here)

1. Drawing
    <div> <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_minibox.png" width="300px"/></div>
2. Photo 
    <div> <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_Minibox-photo.jpg" width="500px"/></div>
</details>
</div>    

<!-- #region -->
#### 3. Alpaca's protection circuit
You will measure two signals
1. Control signal from the DAC that powers the LED
2. Output voltage from the opamp

**These signals can potentially exceed 3.3 V and damage your Cria, so it is essential to attenuate them first. Think about what attenuation you need.**

<details>
<summary><font size=4>ℹ️</font> <b>Hints</b> for the protection circuit</summary>


<div class="alert alert-block alert-warning"> 
    
- Use the knowledge from experiment 12B to configure this protection circuit, 
- Think about what levels you would expect to see in the measurement of the control signal!
- Cria can generate maximum 3.5V and there's a possibility of some persistent positive offet, so it's likely that you can hit the maximum 3.3V. Also, it turns out that it's necessary to apply the the maximum available voltage to the LED to saturate the photodiode, so attentuation is completely justified. 
- The signal of the opamp's output is going to be amplified. The signal will be very high and it will clip at around 12V when the light intesity is high enough to saturate the photodiode.
- In summary, it is wise to go with $1:10$ attentuation for both input channels.

</div>
</details>
<details>
<summary><font size=4>ℹ️</font> <b>Detailed instructions</b></summary>

1. The jumper arrangement is similar to that used in 12B. Use the schematic and the picture below to configure the protection circuit. 
    - Remove the jumpers from both attenuators to set it to $1:10$. 
    - Remember to enable the attenuator by placing wires or jumpers on AMPLIFIER DIRECT TO NANO.
2. Measure the LED signal
    - Connect a wire to the line in the mini-breadboard that hosts: the LED's anode = long leg (+) and wire from the DAC input
    - Connect the other end of this wire to SIGNAL + of the AMP1 IN input.
3. Measure the Output of the opamp
    - Connect a wire to the line in the Alpaca's breadboard that hosts the opamp's output,
    - Connect the other end of ths wire to SIGNAL + of the AMP0 IN input.

</details>

<details>
<summary><font size=4>ℹ️</font> <b>Fritzing of your protection circuit (1:10 attenuation) </b></summary>

<div class="alert alert-block alert-info"> 
<b>Detailed schematics of the 1:10 attenuation </b> <br>
    <div><img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_protection.png" width="500px"/></div>

</div></details>

<!-- #endregion -->

<!-- #region -->
### Part B: Conduct the experiment

#### 1. **Photoconductive Mode**:    
    

**Inquire**

As a reminder of what to expect, here's a quote from the Background section:

>
>- In this mode, the photodiode is grounded **without any resistor** on the input branch, enhancing its responsiveness to light. When light strikes the photodiode, it generates a photocurrent. 
>- By coupling the photodiode to the inverting (-) input of the opamp, we can convert this current into a measurable voltage output. 
>- This mode emphasizes the photodiode's role as a current source, where **the output voltage is linearly related to the light intensity and the feedback resistor**: $V_{out} \propto I_{light} \cdot R_{f}$
>- This means a photodiode generates;
>        - a very low current when illuminated with with low intensity and 
>        - a very high current with high intensity. 
>    - One could say that diode is less sensitive, however its output signal is very suitable for intensity measurements in bio-chemical experiments because of its excellent linearity. 
>    - In measurement applications the photodiode will mostly be used in photoconductive mode (current-mode).
    
    
**Implement**

1. Connect the wire going from the inverting (-) input of the opamp directly to the cathode of the photodiode.
    <div class="alert alert-block alert-info">
    <b>Detailed schematics for the photoconductive mode measurement <br> (Note that the wire to the inverting input is now connected)</b><br>
    Try to <b>compare</b> your circuit to the model circuit and see if there are any significant differences <br>
    <div>
        <details> <summary><font size=4>ℹ️</font> <b>Hints</b> Overview drawing</summary>
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_pc.png" width="700px"/>
        <details>
    </div>
    
    <div>
        <details> <summary><font size=4>ℹ️</font> <b>Hints</b>: Photo of the minibox for the photoconductive mode measurement</summary>
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/miniboxPC.jpg" width="700px"/>
        <details>
    </div>
    </div>
2. Turn on the $\pm 12 \text{V}$ power supply on your Alpaca
3. Review the code below and run the experiment!


<!-- #endregion -->

**Double check if your resistor placement is correct!** <br> **Additionally double check if diode and photodiode are placed correctly in relation to cathode and anode**

```python
# Run this cell to connect your ALPACA
%serialconnect to --port="COM4"
#ADD COM PORT ABOVE, e.g. --port="COM3"

import time
import numpy as np
import matplotlib.pyplot as plt
from machine import ADC
from functiongenerator import FuncGen, Triangle
```

```python
# Do you know what these variables define?
NUM_SAMPLES = 1000
DELAY_MS = 5 # Sampling delay in ms

# How would you figure out which ADC(??) ports to use?
adc0 = ADC(26)
adc1 = ADC(27)

# Remember to initialize the arrays to store the acquired data
LED_control_signal = np.zeros(NUM_SAMPLES) #arrays
Photoconductive_output_signal = np.zeros(NUM_SAMPLES)

# Define the procedure for the function generator and the acquisition
with FuncGen(Triangle(Vmax=3.3, Vmin=0, freq=0.095)):
    for ii in range(NUM_SAMPLES):
        LED_control_signal[ii] = adc1.read_u16()
        Photoconductive_output_signal[ii] = adc0.read_u16()
        time.sleep_ms(DELAY_MS)

# Convert integer input/output signal to voltage.
LED_control_signal = LED_control_signal / 65535 * 3.3 
Photoconductive_output_signal = Photoconductive_output_signal / 65535 * 3.3
print('\n Maximum LED_control_signal: ', np.max(LED_control_signal), '\n', 'Maximum Photoconductive_output_signal: ', np.max(Photoconductive_output_signal))
print('Aquisition done!')
```

```python
# Plot
plt.plot(LED_control_signal, label='LED Voltage Sweep') # you can comment this out if you have big trouble plotting both
plt.plot(Photoconductive_output_signal, label='Output signal in the photoconductive mode') # this is important
plt.ylabel('Signal [V]')
plt.xlabel('Sample index')
plt.legend()
```

#### 2. **Photovoltaic Mode**: 

**Inquire**

Again, as a reminder of what to expect, here's a quote from the relevant Background section:

>    - Alternatively, in the photovoltaic mode, the photodiode operates without any external bias (zero-bias) or also grounded, but with a **load resistor** on the input branch. This operation is similar to a solar cell. Here, it generates a voltage in response to light exposure. 
>    - By again coupling the photodiode to the inverting (-) input of the opamp, this voltage can be amplified and observed as a output voltage. 
>    - In this configuration, the photodiode acts as a voltage source(...), and the output voltage varies with the intensity of the incident light. The voltage generated by the photodiode is again proportional to the light intensity, but the relationship may not be perfectly linear.
>    - **The curve in this case would generally start linear but could show a leveling off (decreasing sensitivity and eventually saturation) at higher light intensities, which is reminiscent of a switch-like behavior or a very sensitive but an inaccurate detector**

**Implement**

1. Move the wire going from the inverting (-) input of the opamp from the position directly at the cathode of the photodiode to include the load resistor.
    <div class="alert alert-block alert-info">
    <b>Detailed schematics for the photovoltaic mode measurement <br>(Note that the wire to the inverting input is now connected)</b><br>
    <div>
        <details> <summary><font size=4>ℹ️</font> <b>Hints</b>:  Overview drawing </summary>
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/12C_pv.png" width="700px"/>
        </details>
    </div>
    <div>
        <details> <summary><font size=4>ℹ️</font> <b>Hints</b>: Photo of the minibox for the photoconductive mode measurement</summary>
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/miniboxPV.jpg" width="700px"/>
        </details>
    </div>
    </div>
2. Do you expect any essential changes in the code? Review the code below and run the experiment!

```python
# Do you know what these variables define?
NUM_SAMPLES = 1000
DELAY_MS = 5 # Sampling delay in ms

# How would you figure out which ADC(??) ports to use?
adc0 = ADC(26)
adc1 = ADC(27)

# Remember to initialize the arrays to store the acquired data
LED_control_signal = np.zeros(NUM_SAMPLES) #arrays
Photovoltaic_output_signal = np.zeros(NUM_SAMPLES)

# Define the procedure for the function generator and the acquisition
with FuncGen(Triangle(Vmax=3.3, Vmin=0, freq=0.095)):
    #time.sleep_ms(100)
    for ii in range(NUM_SAMPLES):
        LED_control_signal[ii] = adc1.read_u16()
        Photovoltaic_output_signal[ii] = adc0.read_u16()
        time.sleep_ms(DELAY_MS)

# Convert integer input/output signal to voltage.
LED_control_signal = LED_control_signal / 65535 * 3.3
Photovoltaic_output_signal = Photovoltaic_output_signal / 65535 * 3.3
print('\n Maximum LED_control_signal: ', np.max(LED_control_signal), '\n', 'Maximum Photovoltaic_output_signal: ', np.max(Photovoltaic_output_signal))
print('Aquisition done!')
```

<div class="alert alert-block alert-info"> 
In the plot below, you should be able see:
    
1. again, the same linearly growing signal driving the LED
2. a <b>non-linear response of the photodiode</b> beyond a certain threshold of the LED intensity.

Think how this response translates to the photodiode acting as a voltage source
    
**Hint**: If the following plot shows text output or is missing data, rerun (several times) the plotting cell with CTRL+ENTER.    
</div>

```python
# Plot
plt.plot(LED_control_signal, label='LED Voltage Sweep') # you can comment this out if you have big trouble plotting both
plt.plot(Photovoltaic_output_signal, label='Output signal in the photovoltaic mode') # this is important
plt.ylabel('Signal [V]')
plt.xlabel('Sample index')
plt.legend()
```

Finally, display both results in one plot and try to think about how these curves correspond to the IV response curve of a photodiode, like the one shown below.

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/IVcurve.png" width="700px"/>
</div>

If the following plot **shows text output or is missing data**, **rerun (several times) the plotting cell** with CTRL+ENTER till you see both lines plotted.

```python
plt.plot(Photoconductive_output_signal, label='Output signal in the photoconductive mode')
plt.plot(Photovoltaic_output_signal, label='Output signal in the photovoltaic mode')
plt.ylabel('Signal [V]')
plt.xlabel('Sample index')
plt.legend()
```

<!-- #region -->
## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**Discuss with TA:**
    
1. List the pros and cons of resistor and opamp based current-to-voltage converters.
2. Reflect on how a photodiode and an opamp can be used to build:
    1. light-controlled current source
    2. light-controlled voltage source
3. Explain why you see a response from the photodiode only at the later stage of the measurement.
4. Reflect on which two different modes of photodiode's operation can be used as 
    1. a sensor
    2. a switch    
5. exit card:
   1. Write a brief abstract on what you learned (conclusion, useful graph),
   2. Which troubleshooting skills do you want to remember for next sessions,
   3. Which code do you copy for use in next sessions,
7. How do think this notebook could be improved
<!-- #endregion -->

```python
#12C opamp current-to-voltage

### TO DO="1. List the pros and cons of resistor and opamp based current-to-voltage converters."

### TO DO="2. Reflect on how a photodiode and an opamp can be used to build current/voltage source."

### TO DO="3. Explain why you see the response from the photodiode only at the later stage of the measurement.

### TO DO="4. Reflect on which two different modes of photodiode's operation can be used as sensor/switch."

### TO DO="5a. abstract"

### TO DO="5b. troubleshooting"

### TO DO="5c. code"

### TO DO="6. what changes would you suggest?"


```

```python

```
