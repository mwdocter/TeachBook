---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.15.2
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
- experiment 3A: build and design RC and RL circuits and observe their behaviour
- experiment 3B: use scope probes and see when it is beneficial to use them
- experiment 3C: cause a ground loop and explain how it is happening

Goal: learn how to design simple filters and how equipment you use affects your measurements

Structure of an experiment:
- Anticipate + Stimulate (5+15+10): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (40): with your partner(group of 2)
- Compare + Conclude (10): with a group of 4(per table)


# EXPERIMENT 3C: ground loop
> <font color='blue'>Learning goal:</font> recognize when equipment you use is affecting your data aquisition


## BACKGROUND:
> <font color='grey'>⏳ Estimated time: 5 min</font>

Next to the signal wires, the connections to ground are equally important. Fun fact: At your home residual circuit breakers are measuring the different in current between supply and ground connection. If the difference is too high, there is a leakage, and the circuit breaker switches off in order to prevent an electric shock. 

You would expect that having many ground connections is a good thing, but it could happen that without any intention, parts of your circuit get connected through ground. So keep an eye on hidden connections, especially through the ground signals in coax cables; you think you connect 1 signal, while in reality you connect 2. 

In this assignment you will unknowingly, but intentionally, will be exposed to hidden connections. Better be prepared. 



## ANTICIPATE: the cause of ground loops
> <font color='grey'>⏳ Estimated time: 15 min</font>


We will sketch the outcome of a measurement, and it is up to you to explain what is actually happening:
* a student builds a voltage divider
* the student connects the function generator with a T connector to channel1 of the scope an Vin of the voltage divider
* the student measures Vout with the scope over the top resistor

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC6d%20probe%20over%20R3.jpg" width=40%></img>

* to the student's surprise Vout=Vin (and not Vin/2)
* the student double checks the output load settings of the function generator (which are correct ... up to you to decide 50 Ohm or HIGH load)

What else could be occurring here?

```python
### TO DO="with the right output load, what is causing Vout=Vin?"

```

<!-- #region -->
## SIMULATE: a circuit that experiences grounding loop
> <font color='grey'>⏳ Estimated time: 10 min</font>


Reuse the simulation from assignment/ week 1C. Instead of measuring over the bottom resistor you will be measuring over the top resistor. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/week3C.JPG" width=40%></img>

* within your simulation draw one additional wire, which represents the hidden connection(to the ground).
* verify that Vout1a-Vout1b with this extra wire is indeed equal to 1V. 


 
<!-- #endregion -->

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python
file_name="3C_1_Circuit_sim.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, works on Vocareum if you change the kernel

Image(filename=file_name, width="50%")
```

```python

```

Feel free to watch the precap (quick intro) on the measurements you will be conduct

```python
#precap
from IPython.lib.display import YouTubeVideo
YouTubeVideo('jR9gxfYlQ80', width = 600, height = 450)

```

## IMPLEMENT & INVESTIGATE: build the circuit and measure unexpected results yourself
> <font color='grey'>⏳ Estimated time: 40 min</font>

Build a voltage divider with R3 and R4 (=1 kOhm) of testboard 1. You should be able to do it on your own. If you need help, check 1C to see how the scheme of voltage divider should look and 3A to see how testboard 1 looks. 
* Connect the probe from oscilloscope to measure the voltage over R3
* Connect the function generator output (using T connector and coax cables) to channel 1 of the oscilloscope, and Vin of the divider circuit.
* Set the generator to a __8 V__ peak-to-peak, __1 kHz sine__ wave with no offset. 
* Measure the voltage across __R3__ using the probe as indicated in the figure below. 

Does the amplitude of the signal correspond with your expectation? **Explain** what goes wrong. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC6d%20probe%20over%20R3.jpg" width=40%></img>

```python
### TO DO="explain what goes wrong with the above measurement?"

```

Solve the problem observed in the previous exercise by removing the BNC-BNC cable between the function generator and CH1 of the scope and measure the voltage across R3 again.

```python
### TO DO="explain why remobing the cable helped"

```

Increase the frequency to 100 kHz and measure the voltage across R3 again and explain whether you are experiencing a ground loop

```python
### TO DO="where does the 'ground loop' at 100 kHz originates from?"

```

To understand this frequency dependent behaviour, have a look at the figure on p. 324 of the manual of the function generator (which can be found on Brightspace). The generator circuit is not grounded directly to the chassis ground, but via a 1M resistor in parallel with a 45 nF capacitance. This is done to eliminate measurement errors due to ground loops as is explained in the text on p. 323-324 of the manual. 
 <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC6e%20Agilent%20scope%20manual.jpg" width=20%></img>
 

<!-- #region -->
## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**
1. Explain what ground loops are, and how they influence your measurement
2. Explain how you can get a frequency dependent ground loop
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
#3C ground loop
### TO DO ="1. explain ground loops"

### TO DO="2. explain frequency dependent ground loops"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python
## classroom equipment recording

from IPython.lib.display import YouTubeVideo
YouTubeVideo('PBeM-u0vjQY', width = 600, height = 450)
```
