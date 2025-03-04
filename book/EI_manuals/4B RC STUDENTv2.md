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
%python
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

<!-- #region -->
Experiments of this week:
- Experiment 4A: Work with fourier transforms on the oscilloscope
- Experiment 4B: Implement an RC circuit with PicoPi
- Experiment 4C: Investigate RCCR circuit with PicoPi and compare to an RLC circuit in LTSpice 


Goal: Work with various types of filters and understand their properties 

Structure of an experiment:
- Anticipate + Simulate (10+10+10): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (30+10): with your partner(group of 2)
- Compare + Conclude (5 min): with a group of 4(per table)

<!-- #endregion -->

<!-- #region kernel="SoS" -->
# Experiment 4B: Implement an RC circuit with PicoPi 

> <font color='blue'>Learning goal:</font> Build the RC circuit and determine the time constant

* Extra information or hints is given in boxes denoted by: <font size=4>ℹ️</font>
* Help in fixing problems is given in boxed denoted by: <font size=4>🔥</font>


Materials used:
- Alpaca
- 22kΩ resistor
- 220 nF capacitor (big orange)

<!-- #endregion -->


## BACKGROUND: Alpaca
> <font color='grey'>⏳ Estimated time: 10 min</font>

Below are two videos that you have already seen in 3A. Watch them if you want to review the topic. 

> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> 
>Notice that in Anticipate there is a different RC filter used. 

```python
## RC low pass filter: already seen in week 3A
%python
from IPython.lib.display import YouTubeVideo
YouTubeVideo('ZwetQNcP0c8', width = 600, height = 450)
```

```python
## Derivation Voltage in an RC circuit (power of e): already seen in week 3A
%python
from IPython.lib.display import YouTubeVideo
YouTubeVideo('jDB2d8zPK9w', width = 600, height = 450)
```

## ANTICIPATE: RC circuit 3dB point
> <font color='grey'>⏳ Estimated time: 10 min</font>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi4_4_RC.jpg" width="500"/>
 
For the circuit above:
* Derive the type of filter
* Calculate the cut-off frequency and time constant 


```python
### TO DO="your answer, type of filter, cut-off frequency and time constant"


```

<!-- #region -->
## SIMULATE: RC circuit 3dB point 
> <font color='grey'>⏳ Estimated time: 10 min</font>

You can download the file called "4_RC_CR_RCCR_Simulate" which should contain an RC circuit that looks like this: 


<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/4B_RC_Simulate_Image.png" width="500"/>


You can run this simulation and by right-clicking on the voltage source you can see that you are performing an AC Analysis. This varies the frequency between 1kHz-10kHz such that you can plot Vout to find the -3dB point. 

> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> 
>The graph plots dB against Hertz. You can determine the frequency by clicking on the name on top of the graph. Also remember that omega=2pi*f
<!-- #endregion -->

```python
### TO DO="What frequency does the -3dB point occur at"


```

<!-- #region -->
You will implement the circuit, do measurements and analyze them. The assignment is broken into two steps: 
1. Implement the circuit and acquire data 
2. Investigate, by analyzing the acquired data

## IMPLEMENT&INVESTIGATE 1: Step function
> <font color='grey'>⏳ Estimated time: 30 min</font>

You will build the RC circuit as below on your Alpaca (220nF is the big flat red capacitor): 
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi4_4_RC.jpg" width="500"/>


    
<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>
    
* Connect the input of the RC circuit ($V_{in}$) to *Dout0*
* Connect the input of the RC circuit ($V_{in}$) to *Ain0* on the multifunction connector (Pin 26)
* Connect the output of the RC circuit ($V_{out}$) to *Ain1*
* Connect the circuit the the ground, on the bottom of your Alpaca

    
<details>
  <summary>Hints for implementation on the Alpaca</summary>
  <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/4B_RC.jpg" width="400"/>
</details>




<!-- #endregion -->

<!-- #region kernel="SoS" -->
We will now use a script that: 
1. Takes 50 samples of the output signal, while the input signal generated by *Dout0* is 0V. 
2. After these 50 measurements, the voltage of *Dout0* is put to 3.3V, and 450 additional samples of the output are performed. 
3. For each measurement, the time at which the measurment was performed is recorded. 
4. The measured output versus the time is plotted.

> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> 
>
> To keep track of when each measurement was taken, consider the following approach: 
> - Record the time at the start of the measurement.
> - Store the time at the start of your measurement in a variable `start`. 
> - Each time you measure, check the time again. Subtracting this time from the start time will yield the time elapsed since the start of the measurement!
> - What do you think will be more reliable: an array that interpolates between the start time and the end time or the measured time?
>
> **How to do this in MicroPython:** As you are measuring using a delay of just 4 milliseconds, you might want to meausure these times in milliseconds too, rather than full seconds. To do this, use `time.ticks_ms`. This gives a "time" in milliseconds. Explore how this works in the code below. Equivalently, instead of `time.sleep`, you can also use `time.sleep_ms` to specify a time to sleep in milliseconds. <font>
<!-- #endregion -->

First change the COM port in the code below and run the code of the next cell. 

```python
%serialconnect to --port="COM4" 
```

```python
#### Explore measuring time points in milliseconds using this code ####
# if you want to explore, change the code from if 0: to if 1:
import time

if 0:
    start = time.ticks_ms() # Current time measured in milliseconds
    tt = [] # List to store time points

    for ii in range(600):
        tt.append(time.ticks_ms() - start) # Store time
        time.sleep_ms(4) # delay

    print(tt[0:50]) #do you see multiples of 4 ms?

```

<!-- #region kernel="SoS" -->
The code is already given to you, you don't have to write it yourself. All you have to do is run it. If you want to get more indepth explanation of the code, feel free to watch the movie below.
<!-- #endregion -->

```python
from machine import ADC, Pin
import time

import matplotlib.pyplot as plt
import numpy as np

from functiongenerator import FuncGen, DC

Ain0 = ADC(26) # Pin 26, Vin
Ain1 = ADC(27) # Pin 27, Vout

from machine import Pin
Dout = Pin(14, Pin.OUT) 

#define the arrays to store data, and activate the pins
Nm = 200 #number of measurements
samples_OUT = np.zeros(Nm, dtype=np.uint16) 
samples_IN = np.zeros(Nm, dtype=np.uint16) 
tt = np.zeros(Nm, dtype=np.uint16)
print("Starting the measurement\n")
start = time.ticks_ms() # Current time measured in milliseconds

for ii in range(Nm):
    samples_OUT[ii]=Ain1.read_u16() # Take a sample
    samples_IN[ii]=Ain0.read_u16() # Take a sample
    
    tt[ii]=time.ticks_ms() - start # Store time
    time.sleep_ms(1)
    
    if ii == 50: # Do this once, and don't forget to turn it off once the loop ends
        Dout.value(True)
        print(ii, "samples acquired\nEnabling Dout0 (Pin GP14)\n")
Dout.value(False)

tt = np.array(tt) # Convert to Numpy array for easy operations
samplesOUT = np.array(samples_OUT)
samplesIN = np.array(samples_IN)


plt.plot(tt, samplesOUT * 3.3 / 65535, '.-', color='b', label='V_out')
plt.plot(tt, (tt > tt[50]) * 3.3, '.-', color='r', label='V_in Theoretical')
plt.plot(tt, samplesIN * 3.3 / 65535, '.-', color='g', label='V_in')
plt.xlabel("Time [ms]")
plt.ylabel("Voltage [V]")
plt.legend()

print(Nm, "samples acquired\nDisabling Dout0\n\nMeasurement is done!")
```

```python
# precap picopi RC (from 2022), with more explanation on the code
from IPython.lib.display import YouTubeVideo
YouTubeVideo('eP-_kH7QfdE', width = 600, height = 450)

```

## IMPLEMENT & INVESTIGATE 2: Time constant 
> <font color='grey'>⏳ Estimated time: 10 min</font>

Above, you have made a plot of the data. We can now determine the time constant $\tau $ experimetaly. Fill in the missing threshold value (???) in the code below to calculate the time constant automatically and print the value to the console. 
> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> 
>The time constant is the time it takes until the signal reaches about exp$^{-1}$ of its maximum value. You can also think about this as a timepoint when the Vout is equal to e$^{-1}$*Vmax. When you input the threshold into the code, it will find for you that timepoint.


```python

voltages= samples_OUT * 3.3 / 65535
idx = np.argmax(voltages) # Index where voltage drop starts, argmax returns the first for multiple maxima

tt_start = tt[idx]
voltages_clipped = voltages[idx:]
tt_clipped = tt[idx:]

#Calculate time constant automatically
'''
Write code here, including defining a threshold=???? in order to find the crossing with that threshold. 
Think about capacitor discharge time and realise you want to find tau=RC, and input the correct formula accordingly as a threshold
Hint: Remember the voltage that was set 
'''

### TO DO="threshold =???, #for you to fill in"


tt_37_percent = tt_clipped[np.argmax(voltages_clipped < threshold)] # get first value where sample > threshold, check out np.argmax
tau = tt_37_percent - tt_start
print(f"The time contant is {tau} miliseconds.")

plt.plot(tt_clipped, voltages_clipped, '.-', color='b', label='V_out [V]')
plt.vlines(tt_start, 0, 3.3, color='grey', label='Dout0 is ON [ms]', linestyle='dashed')
plt.vlines(tt_37_percent, 0, 3.3, color='grey', label='Threshold reached [ms]', linestyle='dashed')
plt.hlines(threshold, tt_clipped[0], tt_clipped[-1], color='magenta', label='Threshold [V]', linestyle='dotted' )
#plt.hlines(threshold, tt_clipped[0], tt_37_percent, color='black', label='RC Time', linestyle='solid' )
plt.xlabel("Time [ms]")

plt.ylabel("Voltage [V]")
plt.legend()

```

```python
print('Summary of the measurement:\n')
print('Samples 50 to 60 acquired from Dout0 [V]:',voltages[50:60])
print('Associated timestamps [ms]:',tt[50:60], '\n')
print('Index of the first sample when Dout0 is enabled:', np.argmax(voltages==np.max(voltages)))
#print('Index of the sample with the (first) recorded maximum value:', np.argmax(voltages))
print('Time when Dout0 is enabled [ms]: ', tt_start)
print('V_out when Dout0 is enabled [V]: ', voltages[np.argmax(voltages==np.max(voltages))], '\n')

print('Threshold value [V]: ', threshold, '\n') 
print('Index of the sample with the measurement below the threshold: ', int(np.argmax(voltages==np.max(voltages))+np.argmax(voltages_clipped < threshold)))
print('Time from start when threshold is reached [ms]: ',tt_37_percent)
threshold_index = int(np.argmax(voltages==np.max(voltages))+np.argmax(voltages_clipped < threshold))
print('V_out when the threshold is reached [V]: ', voltages[threshold_index], '\n')

print(f"The time contant is {tau} miliseconds.")

```

**Do not remove this circuit from your Alpaca, you will need it in the next assignment!!**

<!-- #region -->


## COMPARE & CONCLUDE (first within your group, then with the TA)
> <font color='grey'>⏳ Estimated time: 5 min</font>
* Wait until all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:** 
1. What is the value of the cut-off frequency? 
2. How does the calculated time constant compare to the theoretical value? Please explain the (expected) difference.
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved

**This notebook is on your laptop. To complete the opportunity for TA+teachers, upload this notebook again into Vocareum + submit**
<!-- #endregion -->

```python
### TO DO="1. cut-off frequency"

### TO DO="2. comparison calculated with theoretical time constant"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python


```

```python
%rebootdevice
%disconnect
```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip


```python
## recording 
%python
from IPython.lib.display import YouTubeVideo
YouTubeVideo('Dnm2TuacYDU', width = 600, height = 450)
```

```python

```
