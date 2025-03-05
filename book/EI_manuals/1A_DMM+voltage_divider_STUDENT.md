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
-   experiment 1A: Work with a digital multimeter to measure a voltage divider
-   experiment 1B: Use a function generator to vary the voltage on the load resistance
-   experiment 1C: Design voltage dividers with LTSpice to change the output voltage

Goal: learn how to use function generator, digital multimeter and voltage dividers to measure and control circuits

Structure + Timing of this experiment: 
BASIC = background, anticipate, simulate, implement & investigate, compare&conclude
- 30 (10+10+10+0) min background+anticipate+simulate  : per person. This is homework: finish it before the 4 hours practicum session
- 55 (10+30+15) min implement & investigate: with partner (group of 2)
- 10 min compare&conclude                  : with group of 4 (per table)


# 1A: DMM digital multi meter
(Work with a digital multimeter to measure a voltage divider)
> <font color='blue'>Learning goal:</font> understand how to use the DMM, how it behaves as part of a (voltage divider) circuit, and what its limitations are.

In each assignment the estimated time you will spend per part is given. If you spend much more time than that, it is a clear sign to ask for help sooner. 

## Background 
> <font color='grey'>⏳ Estimated time: 10 min</font>

During this practical session, you will use a power supply, testboard, and two digital multimeters: a handheld one, and one in the caddie (do not take this Agilent DMM out of the caddie!!).
Spend some time getting familiar with these devices. Ask the TAs if you have doubts.

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1a%20DMM.jpg" width=50%></img>

Watch the following movie clips to get introduced to the test boards, power supply and multimeter



```python
## SCR-caddies&testboards

from IPython.lib.display import YouTubeVideo
YouTubeVideo('LwQ_9zIQuX0', width = 600, height = 450)
```

```python
## SCR-power-supply

from IPython.lib.display import YouTubeVideo
YouTubeVideo('qAb6Z6Bv6CQ', width = 600, height = 450)
```

```python
## SCR-DMM

from IPython.lib.display import YouTubeVideo
YouTubeVideo('ykrBCixQ3Lk', width = 600, height = 450)
```

<!-- #region -->

## Anticipate 1: Which voltage will the DMM measure over R2?
> <font color='grey'>⏳ Estimated time: 10 min</font>

If the DMM measures the voltage over R2 in the image below, with R1=R2, predict:
* which voltage over R2 will be measured, for increasing resistor values (both resistors being either 10 Ω, or 1 kΩ, 10 MΩ)?

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1b%20voltage%20divider.jpg" width=30%></img>


Watch the following movie clips for an explanation on the voltage divider
<!-- #endregion -->

```python
## voltage divider

from IPython.lib.display import YouTubeVideo
YouTubeVideo('RSAVxhfckII', width = 600, height = 450)
```

```python
### TO DO ="your answer to predict 1&2"

```

## Anticipate 2: Which setting to use on the DMM for max accuracy
> <font color='grey'>⏳ Estimated time: 10 min</font>

The formula for the accuracy of the 5½ digit DMM is specified as ±(0.025% + 0.005%) = ±(reading%+ range%) by the manufacturer.
 
* Calculate the accuracy for a measurement of 1.2 V on a 2V and 20 V range
* Explain which range is prefered for optimally measuring 1.2V.

> Note: the 1.2V is the reading, which is smaller than the 2 or 20 V range. <br>
> For a 2V range measured with a 3½ digit DMM, the maximum is 1.999 V, the minimum is 0.001 V. <br>
> For a 20 V range measured with a 3½ digit DMM, the maximum is 19.99 V, the minimum is 0.01 V. <br>
> For each range with the same number of digits, you have a different number of digits behind the decimal point. <br>

> **<font color='blue'>__Hint__: </font>**
You'll find these hints throughout the practicals. You can use them to check whether you are on the right track, and for troubleshooting the more common issues.
<br>
For 1.2V, the accuracy is 0.4 mV. (± 0.025% * 1.2+0.005% * 2V)

Please note: 
* not all exercises will be checked/evaluated with the TA, only a selection
* the TA will only check you off after all questions are evaluated (see the section EVALUATE at the end of the notebook)
* you can always ask help from a TA, but before you do, discuss with your direct neighbor (groups of 4); learn from each other.


```python
## accuracy 1.2V in 2V range is
# accuracy 1.2V in 20V range is
# better to measure in the .... range because ....


### TO DO: 'explain which range gives you a better accuracy'

```

## Simulate 
> <font color='grey'>⏳ Estimated time: 0 min</font>

You will have to learn how to simulate in LTSpice, and will learn this in a later assignment. 


## Implement 1: Discuss prediction and simulations
> <font color='grey'>⏳ Estimated time: 10 min</font>

Discuss your prediction and simulations with your partner.
- If they don't agree: find differences, and try to resolve. You may ask your TA for help, always.

Below we continue with the detailed steps for building and measuring the voltage divider. If you want a quick overview, feel free to watch the following movie clip. 

```python
## PRECAP ELC1

from IPython.lib.display import YouTubeVideo
YouTubeVideo('gEYWuJdgVv4', width = 600, height = 450)
```

<!-- #region -->
## Implement&investigate  2: Build a voltage divider with R1&R2 
> <font color='grey'>⏳ Estimated time: 30 min</font>

Make the following circuit on test board 1. 
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1b%20voltage%20divider.jpg" width=30%></img>

### Person1: Connect the power supply with the resistors on the testboard

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC%20testboard1.jpg" width=50%></img>

- Place testboard 1 at your table. 
- Connect a red wire in the red socket, and a black wire in the black socket
- In order to place R1&R2 in series, connect pins U1 (above R2) and U7 (below R1) with a small red wire. 

Connect to the power supply, but do not turn it on yet:
- Connect the other end of the red wire into the red +6V socket of the DC power supply
- Connect the other end of the black wire into the black COM socket of the supply
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1c%20E3630%20dual%20power%20supply%20.jpg" width=50%></img>
Turn the power supply on:
- push the square +6V button (corresponding to the +6V red socket)
- adapt the output voltage to +5V, by turning the +6V knob. 

### Person2: Connect the caddies' DMM as current meter

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC%20front_Agilent_DMM.JPG" width=50%></img>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC%20multimeter%20test%20pin%204XX09_AS01.jpg" width=50%></img>

To measure the current through R1 and R2, replace the small red wire:
- Find a set of wires with test pins at the end
- Connect the red test pin to pin U7 (below R1), the other end to the current input on the DMM.
> Note: the current is measured in series with a circuit, never in parallel. The resistance of a current meter is almost zero, and you might damaged the meter when getting a too high current
- Connect the black test pin to pin U1 (above R2), and the other to the COM port of the DMM

> **<font color='blue'>__Hint:__ </font>**
You can touch the metal part of the resistor to measure above R2 as metal is conductive. The exposed part of pin already in U1 is also metal.

><font color='red'>__WARNING!!__</font>
>* __Measuring current should be done carefully since the DMM is part of the circuit.__
>* __A common mistake is to leave the cables or test leads in the current input terminals and measure voltage.__ 
>* __Remember that the current terminals are connected to a very low resistance (Shunt resistance).__
>* __When connected in parallel to your circuit, high current will flow through the DMM.__
>* __This can damage both the multimeter and the circuit.__
>* __If you are not sure about how to connect your multimeter, please ask one of the TAs.__

- Switch on the DMM, set the measurement to Idc (DC current)
- If the DMM is not responsive: release it to Local (shift button).
> Note: COM stands for common, the return side of all components. For convenience, use black wires to connect to the COM. 

### Person2: Connect the handheld DMM as voltage meter. 
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC%20multimeter%20handheld%20example.jpg" width=20%></img>



- Find another set of wires with testpins.
- Connect the black wire to the COM pin of the handheld DMM
- Connect the red wire to the red Voltge input pin of the handheld DMM
- Connect the black testpin to the bottom of R2 (if you trace the line, you find U14 to be in contact with the black wire)
- Connect the red testpin to the top of R2
- Turn the knob of the handheld to Vdc (or V with a horizontal flat line)

> **<font color='blue'>__Hint:__ </font>**
You can use two DMMs at the same time, one for the current, one for the voltage.  



<!-- #endregion -->

upload your drawing of the meters (or photograph of your circuit+measurement pins) below <br>
To upload the picture, run the first cell of code. Below it, "Upload" button will appear.

Click on it and choose the right file. 
Then run the second cell of code

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload


```

```python
file_name="1A_1_VImeters.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, works on Vocareum if you change the kernel

Image(filename=file_name, width="50%")
```


## Implement&investigate 3: Measure current and voltage of the voltage divider
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Measure the current: read off the value from the DMM. Adapt the range for more significant numbers. 
* Measure the voltage over R2: read of the value from the handheld DMM. Make sure you picked a right setting on the handheld DMM(think your answer to Anticipate 2)
* What do you notice about resistors R1 and R2? 
>    * **<font color='blue'>__Hint:__ </font>**: do they dissipate energy in some way?
>    * **<font color='blue'>__Hint:__ </font>**: holding your hand closeby is enough. Make sure to only touch the casing, not the metal   
    
In the cell below:
* Write down all currents and voltages for all resistor combinations 
* Do the measured values agree with the calculated ones? 
* Is the current through the resistor dangerous? Look it up!

After having measured over R1&R2, you will repeat the measurement, by moving the measurement pins (connected to the current meter). 

* Repeat this with R3&R4, and measure again. Switch roles (person 1 becomes person 2)
* Repeat this with R5&R6, and measure again. 


```python
## TO DO= write down the values for the voltage over R2 and the current through R2 in the divider. 

## TO DO= what do you notice about R1 and R2?


```

<!-- #region -->
## Compare & Conclude: per group of 4
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their measurements, please help each other!
* Compare your results with your other group members. 
* If your results do not agree, or your results are not in line with your predictions and simulations, then work with your group members to find out likely sources of discrepancies. Ask a TA for assistance to help you identify your problem. 

* If your results agree, and are in line with all predictions, then talk to a TA and get checked off


**to be checked off by a TA:**
1. Which setting of the DMM is most accurate (PREDICT2)
2. Explain the odd measured voltage, both qualitatively and quantitatively?
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
# 1A digital multimeter
### TO DO="1.which setting is most accurate (predict2) "

### TO DO="2.explain the odd measured voltage"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code?"

### TO DO="4. what changes would you suggest?"

```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip


```python
## recording ELC1

from IPython.lib.display import YouTubeVideo
YouTubeVideo('Z_bJT63KGFE', width = 600, height = 450)
```

```python

```
