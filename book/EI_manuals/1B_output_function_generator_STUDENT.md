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
-   experiment 1A: Work with a digital multimeter to measure a voltage divider
-   experiment 1B: Use a function generator to vary the voltage on the load resistance
-   experiment 1C: Design voltage dividers with LTSpice to change the output voltage

Goal: learn how to use function generator, digital multimeter and voltage dividers to measure and control circuits

Structure + Timing of this experiment: 
BASIC = background, anticipate, simulate, implement & investigate, compare&conclude
- 28 (8+10+10) min background+anticipate     : per person. This is homework: finish it before the 4 hours practicum session
- 55 (15+15+5+20) min implement & investigate: with partner (group of 2)
- 10 min compare&conclude                    : with group of 4 (per table)


# 1B: Function Generator
(Use a function generator to vary the voltage on the load resistance)

><font color='blue'>Learning goal:</font>
Understand how to connect the function generator, use the proper settings, and measure quantities such as the voltage.

## Background
> <font color='grey'>⏳ Estimated time: 8 min</font>

A function generator is an electronic instrument that generates electrical signals of different waveforms. The most common signals produced are the sine, triangle, square and sawtooth shapes. With this instrument, it is possible to set the amplitude and frequency or your signal to any value in the operation range of the generator. The function generator used in this course has a frequency range of 1 µHz to 20 MHz for sinusoidal waves and an amplitude range of 10 mVpp to 10 Vpp. 
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC2a%20function%20generator.jpg" width=50%></img>

> __<font color='blue'>Hint:</font>__ don't forget to switch the output on!

Watch the following movie clip for an introduction to the function generator

```python
## SCR-function generator
from IPython.lib.display import YouTubeVideo
YouTubeVideo('sig0sRyqE1M', width = 600, height = 450)
```

## Anticipate 1: Distinguish Vpp, Vp, Vrms
> <font color='grey'>⏳ Estimated time: 10 min</font>

While generating a sine wave, the DMM is most often set to Vpp (=Volt peak-peak). The DMM measures the Vrms (=Volt root-mean-square).  

> From a sine wave with amplitude (=Vp=Volt peak) of 1 V, the Vpp is 2V. The Vrms is the continuous DC signal having the same power as the time-varying AC signal, the quadratic mean. The Vrms is 1/sqrt(2)=0.71 V.  

- The function generator generates the signal below. Derive the amplitude of the signal in Vrms, Vp and Vpp. Also derive the frequency:

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC2b%20Vpp%20Vp%20Vrms.jpg" width=50%></img>




```python
#TO DO: what are Vrms, Vp, Vpp, and frequency of the above signal
```

For more information on Vpp, Vp and Vrms, please watch:

```python
## storey:sine wave, visit https://mediaplayer.pearsoncmg.com/assets/58_atE_dMfrjlBJXwgUu6CfZ4iDEgeMO

# from IPython.display import Video
# Video('https://mediaplayer.pearsoncmg.com/assets/58_atE_dMfrjlBJXwgUu6CfZ4iDEgeMO', width = 600, height = 450)
```

## Anticipate 2: Solve how to deal with a range of LOAD resistors
> <font color='grey'>⏳ Estimated time: 10 min</font>

The DMM can be seen as ideal voltage source, with an internal resistance (inside the device). The generated voltage is then divided over the (fixed) internal and the external resistor (termed LOAD resistor; outside the device). 
- Come up with a (qualitative) solution how the function generator (with internal resistor) can work with a wide range of LOAD resistors, for example 50 Ω and 10 MΩ. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1b%20function%20generator%20-%20load%20resistor.jpg" width=30%></img>

```python
## TO DO ; your answers here: 
#1. what is the Vrms, Vp and Vpp of the signal

#2. How can you let a function generator (with internal resistor) work over a large range of load resistors?


```

Before starting the measurements, feel free to watch a brief overview on what you'll be doing



```python
### precap ELC2

from IPython.lib.display import YouTubeVideo
YouTubeVideo('Wvbk_qMJ2kE', width = 600, height = 450)
```

## Simulate
> <font color='grey'>⏳ Estimated time: 0 min</font>

No simulation yet, not until you get properly introduced to LTSpice


## Implement & investigate 1: Measure the Vrms with the DMM
> <font color='grey'>⏳ Estimated time: 15 min</font>

As shown in the diagram below:
* connect the output of the function generator to both the **DMM** and to **test board 1** (making use of a BNC T-connector).

> the BNC connector, and attached coax cable, have two signals. The signal line (inner core of the coax cable) is equivalent to the red wire from the previous exercise. The outer shielding is equivalent to the black wire/ COM. You therefore connect 1 wire, with 2 signal lines. 

* Do not connect the BNC connector to any other components on the test board. 
* Activate the function generator’s output by pressing OUTPUT.
* Program the function generator to generate a sine wave of 1kHz and amplitude 2V
    * set the amplitude to 2 Vrms and measure the voltage with the DMM.
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC2c%20output%20function%20generator.jpg" width=30%></img>

**Questions to answer**:
1. Confirm which type of voltage (Vpp, Vp or Vrms) is measured by the DMM
2. If you input 2 Vrms, does the DMM measure 2 Vrms? 
3. Can you already explain why 2 Vrms from the function generator might not be measured as 2 Vrms by the DMM? 
> At this point: don't worry too much about a possible discrepancy. Please continue - we will investigate further. 

```python
### TO DO="your answers"


```

<!-- #region -->

##  Implement & investigate 2: For which rotary switch setting do you measure the correct Vrms?
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Keep the function generator's output on 2Vrms 
* Connect the rotary switch, a big black turning knob with 12 numbered positions, (SW1) to the BNC. 
* Make sure to form a closed circuit: 
    - use a red wire to go from the signal line of the BNC connector (U30) to the rotary switch (U20)
    - don't forget to connect the BNC's COM (U35) to the other side of the rotary switch
* Measure the voltage over the rotary switch, with the already connected DMM.
* Find the setting (and resistance value) of the switch, for which the voltage measured by the DMM most correct?

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC%20coax%20connectors.JPG" width=30%></img>


>**<font color='blue'>Hint:</font>**
Just measure the Vrms at different Rs as specified by the rotor.
The layout plus values of testboard components can be found on Brightspace, in the document 'Testboard layouts and component values'. THE LAYOUT ABOVE DOES NOT CONTAIN THE RIGHT VALUES FOR THE ROTARY SWITCH
<!-- #endregion -->

```python
### TO DO=" your answer: switch setting .... with resistance value ...."



```

##  Implement & investigate 3: Observe the influence of DC offset on the measured Vrms
> <font color='grey'>⏳ Estimated time: 5 min</font>

* What happens to the measured voltage, when changing the DC offset of the sine wave (at the function generator)?
* Put the DC offset back to 0V.



```python
### TO DO=" your answer: changing the DC offset of the sine wave leads to ....... in the measured voltage "


```

##  Implement & investigate 4: Determine correct use of the output setup
> <font color='grey'>⏳ Estimated time: 20 min</font>

If you know the value of the load resistance, you can use the setting called 'output setup'. This setting will internally switch the voltage supplied with a factor of 2, and allows to deal with a variation of LOAD resistors. 
>* for HIGH Z the function generator assumes an infinite load resistance (for example a DMM). 
>* default (e.g. upon startup), the generator assumes that the load resistance is 50 Ω (for example the rotary switch)

The load setting can be found at the function generator under UTILITY / OUTPUT SETUP, and then select 50 Ω / HIGH Z or LOAD. 
* Do a correct measurement with the testboard connected. 
  - Which LOAD setting should you use? Write down this setting, the Vrms of both function generator and DMM. 
* Disconnect the coax cable from the testboard. 
  - Again: Which LOAD setting should you use? Write down this setting, the Vrms of both function generator and DMM.  

> **<font color='blue'>__Hint:__ </font>**
The OUTPUT SETUP is related to the resistance that is present in the circuit you're measuring.
Think about ALL the components that are in your circuit (... these include the measuring devices).
What is the correct setting? Do you indeed measure the correct voltage?

**Hence, to be sure of the actual signal amplitude, you should always measure it. Keep this in mind during all exercises!**

```python
### TO DO="the correct load setting for the testboard connected and disconnected"


```

> **<font color=red> Optional challenge (exam question)</font>** <br>
> - Predict the measured value for an 1 Vrms output on 50 Ω actual load, while the OUTPUT SETUP is set to high. <br>
> - Predict the measured value for an 1 Vrms output on actual high load, while the OUTPUT SETUP is set to 50 Ω . <br> <br>
> Note: red colored additional challenges are not mandatory, but serve as extra challenge. You can also verify the answers of these additional challenges with the TA

```python
### TO DO="optional: your prediction for measurements with a mismatched load setting"

```

##  Compare & Conclude: per group of 4
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:**
1. Explain how the function generator is able to deal with a large range of load resistors.
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved


```python
#1B function generator output
### TO DO="1. your explanation, to be checked by the TA. DON'T FORGET TO WRITE IT DOWN!"

### TO DO="2a. abstract"

### TO DO="2b. troubleshooting"

### TO DO="2c. code"

### TO DO="3. what changes would you suggest?"

```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip

```python
## recording ELC2

from IPython.lib.display import YouTubeVideo
YouTubeVideo('VZgE8SF8r84', width = 600, height = 450)
```

```python

```
