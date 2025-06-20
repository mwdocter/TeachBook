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

# 11C Diode Bridge

> <font color='blue'>Learning goal:</font> Understand how a rectifier circuit works and what it does for the frequency of the signal

Structure of an experiment:
- Background+Anticipate+ Simulate:  per person; 45 min.
- Implement + Investigate:  with your partner (group of 2); 40 min.
- Compare + Conclude:  with a group of 4 (per table); 10 min.

<!-- #region kernel="SoS" -->
* Extra information or hints is given in boxes denoted by: <font size=4>ℹ️</font>
* Help fixing progrems is given in boxed denoted by: <font size=4>🔥</font>


Materials used:
- Alpaca
- 2 LEDs

## Background
> <font color='grey'>⏳ Estimated time: 5 min</font>

Before building a full wave rectifier, you could read the according book section (see BrightSpace). The half-wave rectifier you saw in the previous experiment already makes a DC signal from AC input. But why only use the positive peaks of the input sine wave if you can also use the negative peaks. But then you should first be able to turn the direction of the negative part of the signal, and make an 'absolute value' of the sine wave. This can be done with a diode bridge, which you'll explore in this experiment. 




<!-- #endregion -->

## Anticipate: diode bridge

> <font color='grey'>⏳ Estimated time: 30 min</font>

A diagram of the **full bridge rectifier** is given below.

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi6_1_bridge_circ1.jpg" width=500/>
</div>

Before you implement it, first think about how the circuit shall respond to an alternating voltage source (Exercise A,B,C). The considered voltage source is a sine wave, with 0 V DC offset. This means the voltage becomes both positive and negative

<!-- #region kernel="SoS" -->
###  A. direction of positive signal
A sine wave is applied at the input of the circuit above (*DAC*). Consider the half of the sine wave when the voltage is **positive**. What is the path via which the current will flow? Note in order which components of the circuit the current passes through.

For example: '*DAC --> Node a --> D1 --> etc.*'
<!-- #endregion -->

```python kernel="SoS"
### TO DO="# write your answere here, describing the path by which the current flows"


```

<!-- #region kernel="SoS" -->
### B.  direction of negative signal
**A)** Consider the half of the sine wave when the voltage is **negative**. What is the path via which the current will flow? Note in order which components of the circuit the current passes through. 

**B)** Do the paths differ between the scenario in Exercise 1 and 2?
<!-- #endregion -->

```python kernel="SoS"
### TO DO="# write your answer here, describing the path by which the negative current flows"


```

<!-- #region kernel="SoS" -->
### C. load= 2 LEDs

**A)** Use the answers above to explain what the output over the *Load* is when an AC sine wave is applied to the input of the circuit above. Illustrate your answer using a sketch.

**B)** If you were to replace the resistor at the load with two LEDs, as in the schematic below, what would their blinking pattern be if a sine wave is applied to the input of the circuit. Explain using your answer from A.

**C)** Discuss the purpose of this circuit.


<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi6_3_bridge_circ3.jpg" width=500/>



<!-- #endregion -->

```python kernel="SoS"
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python kernel="SoS"
import os
file_name="11C_1_sine_diode_bridge.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        f.write(upload.data[-1])
Image(filename=file_name, width="50%")
```

> <font size=4>ℹ️</font> <b>Hints</b> For uploading images, you should switch the kernel of the Jupyter Notebook to Python.
> You can do so via the menu at the top: `Kernel` > `Change Kernel` > `Python 3`


```python kernel="SoS"
### TO DO="# write your answer here, describing the voltage over the LEDs and the blinking (pattern) within a period"

```

<!-- #region kernel="SoS" -->
> <font size=4>ℹ️</font> <b>Hints</b>
> When answering the last predict questions, you might want to:
> 
>* Consider the sign of the signal at the load
>* Consider if the magnitude of the signal at the load is constant or not
>
> <font>   
<!-- #endregion -->

## Simulate: diode bridge
* Build the circuit below in LTSpice and simulate the output voltage, Vout1.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LST5e_rect1b.jpg" width=50%></img>
Note:
* the voltage source is free-floating (you may verify why it is free-floating through simulation).
* Instead of Vin you want to plot the difference between the voltages at both sides of the voltage source (it is free-floating). Add labels to the nodes for your convenience. 
* the wires to the left of D2 and D3 are crossing over, without making a connection (therefore the connecting dot is not there)


```python
### TO DO="your predictions, including what are the max and min values of Vout"


```

```python
upload
```

```python
import os
file_name="11C_third_sim_diode_rectifier.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum
Image(filename=file_name, width="50%")
```

You might want to watch the following movie, especially if your anticipate and simulations did not match: 
* https://www.youtube.com/watch?v=57hoH1HsxZA

<!-- #region kernel="SoS" -->
## Implement&investigate: build & use the rectifier circuit
> <font color='grey'>⏳ Estimated time: 30 min</font>

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi6_3_bridge_circ3.jpg" width=500/>
</div>

<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>

Next, you will implement the full bridge rectifier on the ALPACA. To build the circuit you will use the four LEDs on the ALPACA Playground as diodes. If you feel confident building the circuit you can try this yourself. You are also free to use the instructions below.

    
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi5_5_assistant.jpg" width="50%"/>
</div>
    
<details>
<summary><font size=4>ℹ️</font> <b>Detailed instructions</b></summary>

For the DAC Assistant    
    
1. Connect the *OUT* pin (J16) of the *DAC Assistant* (shown above) to the breadboard. Connect it to the vertical, postive, red-labeled lane (+) which is the second rightmost lane on the breadboard (this corresponds to __node a__ in the circuit above).
    
2. Connect the *GND* pin of the *DAC assistant* (adjacent to the *OUT* pin you connected in the previous step) the vertical, negative, blue-labeled lane (-) which is the rightmost lane on the breadboard  (this corresponds to __node c__ in the figure).

For the LEDS:
    
3. Connect the cathode of *LED-1* to the second rightmost lane/__node a__. So connect *LED1-C* to the positive lane.
4. Connect the second rightmost lane/__node a__ to the anode of *LED-2* (pin *LED2-A*).
5. Connect the output of *LED-3* to the rightmost lane/__node c__. So connect *LED3-C* to the negative (-) lane.
6. Connect the rightmost lane/__node c__ to the input of *LED-4* (pin *LED4-A*).

In order to see the output of this circuit we will set up 2 LEDs as the load. So instead of having a load resistor as in the figure above, we will use 2 red LEDs

Thus we will use the two remaining vertical lanes (+ and - lanes) on the left side of the breadbord for the load: the positive (+ in red) lane as **node b** and negative (- in blue) lane as **node d**. Using these lanes (left side of the breadboard):
    
7. Connect the second leftmost lane/__node d__ to the anode of *LED-1* (pin *LED1-A*).
8. Connect the second leftmost lane/__node d__ to the input of *LED-3* (pin *LED3-A*).
9. Connect the cathode of *LED-2* (pin *LED2-C*) to the leftmost lane/__node b__.
10. Connect the cathode of *LED-4* (pin *LED4-C*) to the leftmost lane/__node b__.
11. Connect the leftmost lane of the breadboard to the *Voltmeter*

12. Connect the load LEDs as shown in the schematic below. For this, place one led with the long leg in the second leftmost lane/__node d__ and the short leg in leftmost lane/__node b__. Place the other LED the other way around. You can use the LEDs you got together with the resistors for this.

13. As last step in building the circuit, connect the _DAC output DAC A_ to the _+IN_ pin of the _DAC assistant_ on the right side of the ALPACA (at J16). 

</details>

> <font size=4>ℹ️</font> <b>Hints</b> The ALPACA manual states that the DAC assistant converts an input from 0 to 4.096 V, to an output voltage range of -10.24 to +10.24 V
>
    
> <font size=4>ℹ️</font> <b>Hints</b> It is best to connect the DAC signal as final step, to make sure no signal is present when you are building the circuit.
>
> <font>   

<details>
  <summary><font size=4>ℹ️</font> <b>Hints  </b>Correct fritzing schematic</summary>
This is how your wiring should look:
    
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi6_4_picopi6_bb.jpg" width=500/>
</details>

<!-- #endregion -->

<!-- #region kernel="SoS" -->
The code for this exercise has already been written and can be found in the cells below. We will apply a 0.5 Hz sine as the input using the DAC.

__No__ measurements will be performed on the Ain channels!! Because the *DAC Assistant* is used, the signal from the DAC is turned into a sine with an amplitude from +10 V to -10 V. 

Check whether your answer to ANTICIPATE C. was correct by excecuting the code below.
<!-- #endregion -->

```python kernel="MicroPython - USB"
%serialconnect to --port="COM3" 
#ADD COM PORT ABOVE, e.g. --port="COM3"

import time
from functiongenerator import FuncGen, Sine # type: ignore

with FuncGen(Sine(Vpp=4, offset=2, freq=0.5, unsafe=True)):
    
    time.sleep(10)
```

> <font size=6>🔥</font>  
>
> <font color='ff6723'>
>
> If you are not getting any ouput, please verify that:
> * Both USB cables are plugged in
> * The _+12 V_ and _-12 V_ switches are turned to the on position on the ALPACA. 


The ALPACA doesn't have a floating source (voltage is grounded, unlike the circuit in SIMULATE), so the voltmeter goes into negatives while you run the code, this is normal.


How could you improve this circuit to generate DC voltage
> <font size=4>ℹ️</font> <b>Hints</b>
> Think back to 11B (the previous diode assignment): What did you do there to create a stable the output voltage?



```python
upload
```

```python

file_name="11C_2_sketch_improved_circuit_output.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        f.write(upload.data[-1])
Image(filename=file_name, width="50%")

```

> <font size=4>ℹ️</font> <b>Hints</b> For uploading images, you should switch the kernel of the Jupyter Notebook to Python.
> You can do so via the menu at the top: `Kernel` > `Change Kernel` > `Python 3`


<!-- #region kernel="SoS" -->
## Compare & conclude
> <font color='grey'>⏳ Estimated time: 5 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**

1. Can this circuit now be used as a DC power source? If not, what should be done to make this possible (supply your argumentation with the relevant formula and a sketch of the resulting output)? If so, explain why?

2. exit card:
   1. Write a brief abstract on what you learned (conclusion, useful graph),
   2. Which troubleshooting skills do you want to remember for next sessions,
   3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved

<!-- #endregion -->

```python kernel="SoS"
#11C full bridge rectifier
### TO DO="# 1.Explain why and how you can use it as DC power source"

### TO DO="2a. abstract"

### TO DO="2b. troubleshooting"

### TO DO="2c. code"

### TO DO="3. what changes would you suggest?"

```

```python kernel="MicroPython - USB"
%use micropython
%rebootdevice
```

```python kernel="MicroPython - USB"
%use micropython
%disconnect
```

* Recording: https://www.youtube.com/watch?v=dTXTBHwkaac

