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
-   experiment 11A: make diode clippers
-   experiment 11B: build diode rectifier
-   experiment 11C: build a diode bridge

Goal: learn how to apply diodes in clippers and rectifiers

Structure of an experiment:
- Background+Anticipate+ Simulate:  per person; 45 min.
- Implement + Investigate:  with your partner (group of 2); 40 min.
- Compare + Conclude:  with a group of 4 (per table); 10 min

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-21510b084dded588", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
# 11B: Diode rectifier

> <font color='blue'>Learning goal:</font> Explain and understand fundamental properties of diode circuits 


* Extra information or hints is given in boxes denoted by: <font size=4>ℹ️</font>
* Help fixing progrems is given in boxed denoted by: <font size=4>🔥</font>



Materials used:
- ALPACA
- A diode
- A capacitor of 47 $\mu$F
- A resistor of 1k $\Omega$
- A red LED

## Background: LED and rectifier
> <font color='grey'>⏳ Estimated time: 5 min</font>

During this exercise you will explore the functioning of a diode in different circuits. For this purpose, you will mostly be using a specific type of diode called a **Light Emitting Diode** (LED). An LED functions just like any other diode, with the added functionality of creating light when a current flows through it. The ALPACA has many LEDs already available on board, and you'll use these in circuits. A difference between a LED and other diodes is the voltage drop - LED's is higher at around 2V (depending on a LED type and manufacturer).


You already know that a capacitor has a frequency dependent impedance. It 'blocks' low frequency signals. When combining a clipper with a capacitor, you can make use of the frequency dependent impedance, and build a rectifier. In this experiment we will look at a half-wave rectifier, which makes a DC signal from AC input. 
<!-- #endregion -->

<!-- #region -->
## Anticipate: LEDs and a Rectifier

**A)** Predict the blinking behavior of the LED in the circuit below with an input (at *DAC A*) of a square wave with a $V_{min}$ of 0 and $V_{max}$ of 1 V.
Explain which of the following answers is correct:
- no led will turn on
- only led A will blink
- only led C will blink
- both leds blink at the same time
- the leds blink alternatingly (either one on at any point in time)
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_2_circ_D2.jpg" width="400"/>
</div>

**B)** Do the same prediction but with with a different input signal: $V_{min}$ of -10 V and $V_{max}$ of 10 V.



**C)**  what will be the output voltage, Vout0 ?

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LST5e_rect1a.jpg" width=50%></img>

For the exercise C you might want to read the rectifier book section (see scheme on BrightSpace)
<!-- #endregion -->

```python nbgrader={"grade": true, "grade_id": "cell-c7b4ad76d0639d43", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
### TO DO ="your prediction + reasoning"

```

Note: In a half wave rectifier only half of the sine wave passes through a diode, charging the capacitor. In the rest of the remaining period the capacitor will discharge, leading to the characteristic output signal. 


## Simulate: half-wave rectifiers in LTSpice
> <font color='grey'>⏳ Estimated time: 15 min</font>
* Put the above rectifier circuit (**C**) in LTSpice. Insert your screendumps of the LTSpice circuit+simulated signals.


```python
from ipywidgets import FileUpload
from IPython.display import Image

upload=FileUpload()
upload
```

```python
import os
file_name="LTS05e_third_sim_diode_rectifier.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum
Image(filename=file_name, width="50%")
```

The following movie gives more info for the simulation.

```python
%python
# diode rectifier 1b
from IPython.lib.display import YouTubeVideo
YouTubeVideo('Pcwxl9ofFHE', width = 600, height = 450)
```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-9c7393b569650415", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
## Implement & investigate 1: directionality and polarity of LEDs
> <font color='grey'>⏳ Estimated time: 25 min</font>

   
### 1a Implement the circuit 
You can use the LEDs that are present on the Alpaca to test the directionality and the polarity. Implement the circuit below.

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_1_circ_D1.png" width="50%"/>
</div>

<details>
  <summary>Detailed fritzing</summary>
  <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_2_build.jpg" width="70%"/>
</details>

In order to test this circuit you will write some Python code. Part of it is al ready written, but you will have to complete it. The code can be found in the cells below. 
    
* The code must be able to make a square wave ranging from 0 to 3 V. For the frequency, we will select a frequency at which we can easily see the changes in the LEDs by eye, say 1 Hz. You can do this using the function generator and the *DAC A* output. You must be able to set a certain frequency for this square wave. 
* Do **not** yet perform a measurement using the analog in pins during this first part of the exercise.

Part of the code is already written for you in the cells below. Comments with instructions are given at the places where you should complete it.
    
> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> To refresh your memory, the syntax for the function generator was:
> ```python
> from functiongenerator import FuncGen, Square
> with FuncGen(Square(Vpp=X, offset=Y, freq=Z)):
>     # Do stuff here
> ```
>
>    <font>    
<!-- #endregion -->

```python kernel="MicroPython - USB"
%serialconnect to --port="COM3" 
#ADD COM PORT ABOVE, e.g. --port="COM3"
```

```python kernel="MicroPython - USB" nbgrader={"grade": true, "grade_id": "cell-bd0e787340aa9b80", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
import time
from functiongenerator import FuncGen, Square

'''
Fill in a desired frequency and a duration
'''
FREQ = None     # Frequency of the measurement, in Hz
DURATION = None # Duration of the measurement, in seconds


'''
Add the correct Vpp, offset, and frequency
'''
with FuncGen(Square(Vpp=None, offset=None, freq=None)):
    '''
    Think of how we can have the square wave be on for the time specified by DURATION.
    '''
    pass
```

```python kernel="MicroPython - USB"

```

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-ae93836757f43352", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> Thinking of what to put inside the `with` statement?
> Remember that the `with` function stops whenever the code inside the
> `with` (so whatever is indented) is finished.
>
> <font>    

### 1b describe the LED blinking pattern
Describe the blinking patterns of the LEDs (are they alternating or do they blink simultaneously)? Is this what you expect, note that the orientation of both LEDs is the same (unlike the above prediction)?
<!-- #endregion -->

```python kernel="SoS" nbgrader={"grade": true, "grade_id": "cell-26f81ce178351053", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
# Describe the blinking patterns of the LEDs(are they alternating or do they blink simultaneously)? Is this what you expected?
# write your answer here

### TO DO="Describe the blinking patterns of the LEDs. Did you expect this pattern?"


```

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-fc62f2db3a7e2228", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>
    
* You are going add a slight change to the circuit now. Implement the circuit below. Note that you only have to switch the wires on LED3-A and LED3-C in order to build the new circuit.
* You can use the same code you wrote in the previous exercise. Keep the frequency at 1 Hz.

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_2_circ_D2.jpg" width="50%"/>
</div>


### 1c blinking for different LED orientations compared
**A)** Check you prediction for two LEDs with opposite orientation (exercise 1A)
    
**B)** Observe how the blinking pattern changed compared to the previous exercise. 
    
**C)** Explain the difference between the blinking patterns of exercise 1 & 2?
<!-- #endregion -->

```python kernel="SoS" nbgrader={"grade": true, "grade_id": "cell-898153a6ddbbd47c", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
### TO DO="explain the differences between blinking in 1a and 1b "


```

<!-- #region kernel="MicroPython - USB" nbgrader={"grade": false, "grade_id": "cell-3b5b65a9423ebb4e", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> Did you expect both LEDs to blink in an alternating fashion? Why did that not happen?
>
> <font>    
<!-- #endregion -->

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-202a4d644a545fe4", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
### 1d signal through DAC assistant
    

Let's change the circuit a slight bit again . Instead of using the output from the *DAC A* pin directly, we pass it through the *DAC Assistant* on the ALPACA. 
* Wire the signal of pin *DAC A* to the *+IN port* of the DAC assistant (right top of the ALPACA board, see image below). The output signal is now generated at the pin called *OUT*. Also see the circuit below.
* Note that for this part of the exercise both USB cables must be connected. Turn the two switches for the *+12V >ON* and *-12V >ON* supplies on, their indicator LEDS will glow now.
    
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_5_assistant.jpg" width="50%"/>
</div>


> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> The ALPACA manual states that the the DAC assistant converts an input from 0 to 4.096 V, to an output voltage range of -10.24 to +10.24 V
>
> <font>    
    
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_3_circ_D3.jpg" width="50%"/>
</div>



Because you're using the DAC assistant on the ALPACA, the square wave you create using the function generator will be turned into a square wave of the same frequency, but with a voltage from -10 to +10 V. You can see that your voltage range changed by using the voltmeter on the Cria, __but avoid connecting your signal to any of the input pins of the Pico__, this voltage range is harmful for your it!

    
* Observe the blinking pattern of the LED by running the code you wrote previously. What happened, and can you explain why this happened?
<!-- #endregion -->

```python kernel="SoS" nbgrader={"grade": true, "grade_id": "cell-2c9794e372f3880f", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
### TO DO="describe and explain the blinking"


```

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-26fc986164912f94", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
### 1e frequency of the LED
Increase the frequency in the code. Determine the highest frequency at which you can still observe the blinking of the LEDs. Why can you no longer seperate the blinking of the LEDs at high frequencies?
<!-- #endregion -->

```python kernel="SoS" nbgrader={"grade": true, "grade_id": "cell-35e55834d0d65411", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
### TO DO="for which frequency you cannot see blinking anymore, and why?"

```

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-b282da7c3283c233", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
## Implement & investigate 2: half-wave rectifier

### 2a. build the setup
> <font color='grey'>⏳ Estimated time: 15 min</font>

Now we are going to build the last circuit for this exercise. 
* Unplug your ALPACA. (Always recommended when rebuilding)
* For this purpose, do not use one of the LEDs on the ALPACA, but **take the red LED that was supplied to you** together with the capacitors and resistors.
* Note that you'll no longer be needing the *DAC Assistant*
    
Build the circuit below using the breadboard on the ALPACA. 

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_4_circ_D4.jpg" width="50%"/>
</div>
    
<details>
  <summary>Detailed fritzing</summary>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_5_build.jpg" width="70%"/>
</details>

    
### 2b. do the measurement  
The code for this exercise is already written for you, it generates a sine wave. Execute it by running the cell below. Both the input and output signal of the circuit will be measured.

> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> Remember that the long leg of the capacitor and the LED correspond to the positive side (anode).
>
> <font>   
<!-- #endregion -->

```python kernel="MicroPython - USB"
%serialconnect to --port="COM3" 
#ADD COM PORT ABOVE, e.g. --port="COM3"

import time
import numpy as np
from machine import ADC
from functiongenerator import FuncGen, Sine

NUM_SAMPLES = 100
DELAY_MS = 1

adc0 = ADC(26) 
adc1 = ADC(27)

input_signal = np.zeros(NUM_SAMPLES)
output_signal = np.zeros(NUM_SAMPLES)

with FuncGen(Sine(Vpp=3, offset=1.5, freq=20)):
    
    time.sleep(0.5)

    for ii in range(NUM_SAMPLES):
        input_signal[ii] = adc0.read_u16()
        output_signal[ii] = adc1.read_u16()
        time.sleep_ms(DELAY_MS)

input_signal = input_signal / 65535 * 3.3
output_signal = output_signal / 65535 * 3.3
```

```python kernel="Python3"
import matplotlib.pyplot as plt


plt.plot(input_signal, label='Input signal')
plt.plot(output_signal, label='Output signal')
# plt.plot(output_signal + 0.7, label='Output signal + 0.7 V')
plt.ylabel('signal [V]')
plt.xlabel('Measurement number')
plt.legend()
```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-a877ae5c2691a5b4", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
> <font size=6>🔥</font>  
>
> <font color='ff6723'>If the values of the input/output signal are lower than you expect, please check that
> there are **no** jumpers on *AMPLIFIER DIRECT TO NANO* (slot on the right of the Cria-Pico Pi converter)
</font>

You might have noticed that the output is lower than you might expect. That has to do with us using LEDs instead of simple diodes. LEDs have a higher voltage drop.
<!-- #endregion -->

<!-- #region kernel="SoS" nbgrader={"grade": false, "grade_id": "cell-8dceb7a380d846a6", "locked": true, "schema_version": 3, "solution": false, "task": false} -->

### 2c explain plus test the dependency on the capacitor value
**A)** Explain the shape of the output signal 

**B)** Test what will happen with the ripple voltage if you increase the capacitance of the capacitor


<!-- #endregion -->

```python kernel="SoS" nbgrader={"grade": true, "grade_id": "cell-cb5820b3ba05c6fc", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
### TO DO="Explain the shape of the output signal, how does this depend on the capacitance value?"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-8f8eae4820cc6648", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
## Compare and conclude
> <font color='grey'>⏳ Estimated time: 10 min</font>


* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**
1. explain why this rectifier is a half-wave rectifier?
2. For the half-wave rectifier, discuss the effect of a (bigger) capacitor.
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python nbgrader={"grade": true, "grade_id": "cell-a48813197e74e36a", "locked": false, "points": 1, "schema_version": 3, "solution": true, "task": false}
#11B diode rectifier
### TO DO ="1. explanation why this rectifier is a half-wave one"

### TO DO="2. Explain the effect of a larger capacitor"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python kernel="SoS"
%rebootdevice
```

```python kernel="SoS"
%disconnect
```

```python
%python 
# recording
from IPython.lib.display import YouTubeVideo
YouTubeVideo('YevPm66rgGg', width = 600, height = 450)
```
