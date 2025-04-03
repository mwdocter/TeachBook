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
-   experiment 11A: make diode clippers
-   experiment 11B: build diode rectifier
-   experiment 11C: build a diode bridge

Goal: learn how to apply diodes in clippers and rectifiers

Structure of an experiment:
- Background+Anticipate+ Simulate:  per person; 55 min.
- Implement + Investigate:  with your partner (group of 2); 50 min.
- Compare + Conclude:  with a group of 4 (per table); 10 min.

<!-- #region nbgrader={"grade": false, "grade_id": "cell-52fd721867965ee1", "locked": true, "solution": false} -->
# 11A - Diode Clippers
><font color='blue'>Learning goal:</font> understand how diodes can lead to clipping of a signal and how to recognize these signals in a classroom setting
<!-- #endregion -->

<!-- #region nbgrader={"grade": false, "grade_id": "cell-6c1c1d9274495e8b", "locked": true, "solution": false} -->
Materials used:
- Test board 1
- Function generator
- Oscilloscope


<!-- #endregion -->

## BACKGROUND Clippers:

Read the appropriate sections of the book (see schedule on BrightSpace), and watch the following video 

```python
##diode basic explanation
from IPython.lib.display import YouTubeVideo
YouTubeVideo('4O4Qf2TgXwU', width = 600, height = 450)
```

Following are the highlights of the video in text, read them carefully. 

A diode is a polarity sensitive switch, the plus side is called anode, the minus side is called cathode. Only if the **voltage at the anode is higher than at the cathode, the diode is conducting**. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS5e_diode_anode_cathode.JPG" width=30%></img>

The above description is a simplified model of the diode. A better approximation is the following, with a voltage drop of 0.7 V over the diode, and a linear I-V charateristic for V larger than the voltage drop, modeled by R_on of 10 Ohms.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS5e_diode_approximation.JPG" width=60%></img>



Although the behavior of a diode is simple, diode circuits can be really complicated. In this simulation exercise we will look at a number of circuits, most of them you will explore during the practical assignments.

## ANTICIPATE 1: simple diode circuit
> <font color='grey'>⏳ Estimated time: 15 min</font>

In the following schematic only one resistor, one diode and a voltage source is used. Predict Vout0 and Vout1 through the following steps:
* what are the voltages on both sides of the diode?
* when starts the diode conducting?
* what will be the output voltage?
* when will there be current through the diode?
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS5e_two_simplest_diode_circuits.JPG" width=100%></img>

```python
### TO DO="your predict1 for the above two circuits"


```

## ANTICIPATE 2: Advanced clipper circuit
> <font color='grey'>⏳ Estimated time: 10 min</font>

Diodes can be used together with a voltage divider or with a capacitor in series of the voltage source:

Similar to the previous prediction, answer the following questions for Vout3 and Vout4:
* when starts the diode conducting?
* what will be the output voltage?

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS5e clipper advanced.jpg" width=50%></img> 
Vin is a sine with 16 Vpp (or 8 Vp), and 1 kHz


```python
### TO DO="your predict2 for the clipper "


```

## SIMULATE 1: simple diode circuit in LTSpice

> <font color='grey'>⏳ Estimated time: 10 min</font>

Put the two 'simple' circuits in LTSpice. Insert your screendumps of the LTSpice circuit+simulated signals.

```python
from ipywidgets import FileUpload
from IPython.display import Image

upload=FileUpload()
upload
```

```python
import os
file_name="11A_first_sim_diode.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, change kernel in Vocareum!
Image(filename=file_name, width="50%")
```

```python
##simplest diode circuit -->NEW MOVIE?
from IPython.lib.display import YouTubeVideo
YouTubeVideo('Hbvfnd9TpnU', width = 600, height = 450)


```

## SIMULATE 2: Advanced clipper in LTSpice
> <font color='grey'>⏳ Estimated time: 10 min</font>

 Put the 'advanced' clipper circuit in LTSpice. Insert your screendumps of the LTSpice circuit+simulated signals. 


```python
upload
```

```python
import os
file_name="11A_second_sim_clipper.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, change kernel in Vocareum!
Image(filename=file_name, width="50%")
```

```python
##diode clipper -->NEW MOVIE?
from IPython.lib.display import YouTubeVideo
YouTubeVideo('dQ7I-PZB7Rg', width = 600, height = 450)
```

Compare the 'simple' and 'advanced' circuit behaviour. What is the advantage of the complex circuit?

```python
### TO DO="your answer on benefit complex circuit"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-07cab9b6699ce364", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
## IMPLEMENT & INVESTIGATE 1: Clipping 
> <font color='grey'>⏳ Estimated time: 25 min</font>

**Remember to set the right output setup on the function generator**

### 1a. clipping based on 1 diode

First we will have a look at some clipper circuits. In the figure below we see a clipper, based on one diode.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC9e-1.jpg" width=70%></img>

* Build the circuit and make sketch (or photo) of the measured input and output signals (1Vpp and 10Vpp sinewave, 1 kHz). Make sure the inputs of the scope are set to DC-coupling.

> **<font color ='blue'> Hint: </font>**
What is the maximum voltage that can be over the diode? If you get a different output than you expected, check the direction of the diode and whether the scope is set to DC-coupling.
<!-- #endregion -->

 insert your image(s) below

```python
upload
```

```python
file_name="11A-1_sketch.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

```python
### TO DO="further comments on your sketch"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-53490fd3193880e0", "locked": true, "solution": false} -->
### 1b. Clipper based on two diodes

Next the circuit can be expanded with an extra diode **in anti-parallel configuration**, as shown in this figure. R4 is 1 kOhm.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC9e-2.jpg" width=70%></img>

* Build the circuit and make sketch of the measured input and output signals (1Vpp and 10Vpp sinewave). Make sure the inputs of the scope are set to DC-coupling.
<!-- #endregion -->

<!-- #region nbgrader={"grade": false, "grade_id": "cell-4adbcd47c1d21eb3", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
 insert your image(s) below
<!-- #endregion -->

```python
upload
```

```python
file_name="11A-2_sketch.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

```python
### TO DO="further comments on your sketch"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-d0915cf0e61e6080", "locked": true, "solution": false} -->
* Have a look at the FFT of the above two signals. What do you observe when going from 1Vpp to 10 Vpp? 
<!-- #endregion -->

```python nbgrader={"grade": false, "grade_id": "cell-ac00adfa79c75640", "locked": false, "schema_version": 3, "solution": true, "task": false}
### TO DO="# what do you observe for the FFT for 1Vpp and 10 Vpp"

```

 insert your image below

```python
upload
```

```python
file_name="11A-3_not_clipping.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-cbc2313e1e8d25c7", "locked": true, "solution": false} -->
## IMPLEMENT & INVESTIGATE 2: Advanced clipping
> <font color='grey'>⏳ Estimated time: 25 min</font>
### 2a. Clipper based on one diode in the signal-path

In all the previous circuits the diode is placed between ground and the signal path. In another category of clippers, the diode is placed in series with the signal path. Now we will have a closer look at such a circuit.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC9e-4.jpg" width=50%></img>

The connectors J2 and J3 will be connected to the power supply (set to +5V). J2 to the positive terminal, J3 to the negative terminal(COM). 
Vin=16 Vpp, 1kHz. R3=R4= 1kOhm 

* Build the circuit and measure the input and output signals. 
* Make a sketch of the waveforms. 
* Do the waveforms cohere with your expectations? Make sure the inputs of the scope are set to DC-coupling.
<!-- #endregion -->

<!-- #region nbgrader={"grade": false, "grade_id": "cell-745eefb6c5caba25", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
insert your image below
<!-- #endregion -->

```python
upload
```

```python
file_name="11A-4_clipper.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

```python
### TO DO="further comments on your sketch/photo"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-56162c813dd4ef5a", "locked": true, "solution": false} -->
 ### 2b. reverse diode direction
 Modify the circuit as in the figure below (swap the diode connections).

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC9e-5.jpg" width=50%></img>

* Measure the output signals. 
* Is this output-signal as expected? 
* Make a sketch of the output-signal.
<!-- #endregion -->

<!-- #region nbgrader={"grade": false, "grade_id": "cell-e940b99b44e0fbc5", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
insert your image below
<!-- #endregion -->

```python
upload
```

```python
file_name="11A-5_clipper_diode_inverted.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-7a8d2708e4cea3a6", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
> **<font color = 'blue'> Hint: </font>**
If you get stuck on predicting/explaining the signal, try to write down what the DC voltage with respect to the ground is at the different pins (e.g. J2, U26, U14) and what this means for the diode.
<!-- #endregion -->

```python
### TO DO="further comments on your sketch/photo"

```

Now move the power supply knob slowly. What do you observe on the scope?

```python
### TO DO="your observation, what changes with varying the input voltage?"

```

<!-- #region -->
## COMPARE&CONCLUDE
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**

1. in your own words, explain (to the TA) what is a clipper is, and its purpose?

2. Name a benefit of each of the below circuits:

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC9e_clippers.jpg" width=80%></img>

3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
#11A diode clipper
### TO DO='1 explain what a clipper is'

### TO DO="2 Name a benefit of each clipper circuit"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python
##recording
from IPython.lib.display import YouTubeVideo
YouTubeVideo('NlsTgr56bT0', width = 600, height = 450)

```

```python
%rebootdevice
```

```python
%disconnect
```
