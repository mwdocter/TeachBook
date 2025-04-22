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
- Experiment 7A: Use NI-DAQ to acquire analog signals 
- Experiment 7B: Write a function that calculates the fourier transform 
- Experiment 7C: Receive and decode Morse code

Goal: Acquiring analog signals with NIDAQ, plus writing and testing code for the Fourier transfrom and to decode the Morse signal (from 6B)

Structure of an experiment:
- Background+Anticipate (+ Simulate) (40 min): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (50 min): with your partner(group of 2)
- Compare + Conclude (5 min): with a group of 4(per table)



# 7A: Acquire Analog Inputs

> <font color='blue'>Learning goals:</font> You will learn how to acquire analog signals using a Data Acquisition (DAQ) board. 

In a previous exercise, you confirmed that the NI-DAQ board was configured correctly.
Again, since the DAQ board requires special drivers, you cannot run this exercise in Vocareum. **Therefore, you have to download this module and run it locally (for example in Anaconda Jupyter Notebook) instead of using Vocareum.** Ask your TA for help, in case you do not manage on your own. 

REMEMBER TO SAVE THE FILE IN H: DRIVE, NOT C: DRIVE


## BACKGROUND: 
> <font color='grey'>⏳ Estimated time: 10 min</font>

An analog signal is continuous and smooth. They may have a limited range of values such as the sine wave (which is an analog signal) shown below, however there are still an infinite number of possible values within this range. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PyDAQ-Next/7A_Sinewave_background.jpg" width=40%></img>

On the other hand digital signals, as you have worked with last week, have only a finite set of possible values. Most commonly digitial signals will have two possible values such as 0V or 4V. An example of this digital signal is shown below 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PyDAQ-Next/Digital%20signal%20example%20background.png" width=50%></img>

In this assignment you will take digital measurements of an analog signal. Depending on the bit depth of the Analog to Digital converter (ADC), the difference between the measured digital and actual analog input value can be smaller or larger. For better accuracy higher bit depth is better, but then your data set is larger/ will need more memory on the computer. <br>
In the timing you also have some kind of accuracy. If you take one measurement of a constant value, the exact time of acquisition does not matter. But what if the input signal is time varying, like a sine. Then you should think about how to sample properly in order to have the measured (digital) signal to resemble a sine as well.<br>
You'll explore this concept in this assignment. 


## ANTICIPATE: Most accurate measurement of a time-varying signal
> <font color='grey'>⏳ Estimated time: 20 min</font>

In this practical we will take two approaches to record a sine wave with the DAQ board: 
1. A for loop over a single measurement. (With `time.sleep()` we will add a delay in between measurements) = `for` loop
2. A multipoint measurement( a sequency of measurements) at predetermined time points. = Timed acquisition

For you to answer:
* Argue why both approaches should give the same outcome?
* Argue why both approaches give a different outcome, especially for little to no waiting time?
* What do you think will happen, same outcome or not?

> ### <font color='blue'>Hints:</font>
> * does a one point measurement take considerable amount of time?
> * do processes running at the computer's bakcground take considerable amount of time?

```python
### TO DO="your arguments plus prediction"

```


The importance of this might be hard to see when when doing not precise measurements. 
If you want to explore this topic more, feel free to visit https://pynative.com/python-get-execution-time-of-program/


Feel free to watch this 2022 precap, which introduces you to the Analog input code

```python
#precap
from IPython.lib.display import YouTubeVideo
YouTubeVideo('SutAT5PpFqw', width = 600, height = 450)
```

## SIMULATE: already work on the code of I&I1
> <font color='grey'>⏳ Estimated time: 10 min</font>


## IMPLEMENT & INVESTIGATE 1: Single channel with a for loop
> <font color='grey'>⏳ Estimated time: 30 min</font>

**Exercise 1: Single acquisition**

To get you acquainted with the analog inputs of the DAQ board, we will first take a single voltage measurement.

For interacting with the DAQ board, we will use `nidaqmx`. 
* You can find some documentation about this package here: https://nidaqmx-python.readthedocs.io/en/latest/.
We will also use some other packages. That some time to look at imported libraries in the code below.

Connect the function generator output to Dev1/ai0. Set it to a sine wave with a frequency of 1 Hz and an amplitude of 1 V. 
Run the following cell:

```python
## Import all required libraries/ functions
import nidaqmx
import numpy as np
import matplotlib.pyplot as plt
import time
from nidaqmx.constants import AcquisitionType, Edge, LineGrouping

## Defining a system for nidaqmx. We will assign a DAQ task to it later on.
system = nidaqmx.system.System.local()
system.driver_version # Uncomment if you want to see the driver version

# Define a task, assign a channel, do a measurement
with nidaqmx.Task() as task:
    task.ai_channels.add_ai_voltage_chan("Dev1/ai0")
    measurement=task.read()

print(measurement)
```

As you see, one measurement is not enough to grasp an alternating voltage. Therefore, we will now develop a way to measure multiple samples after each other .

**Exercise 2: Multiple acquisitions using a for-loop**

To do multiple acquisition, we will try to recycle the previous code in a `for` loop.

Complete this piece of code into a for loop that measures 200 samples.

```python
number_of_samples = 200
measurement = ...... # initialisation where the measurements will be stored, np.zeros()
# Define a task, assign a channel, do a measurement
with nidaqmx.Task() as task:
    
    # add the channel "Dev1/ai0"
    task. .........
    
    
    for ii in range(......):
        
        # measure a sample with task.read(), assign it to something with measurement
        measurement.........
        

# plot measurement
.........
```

```python

```

Look at your plot. Does it resemble the expected sine wave that you entered into the function generator?

Most likely not, because you did not pay attention to the timing settings.

You should define a certain sampling rate and the total number of samples. 

## IMPLEMENT & INVESTIGATE 2: Timed acquisition
> <font color='grey'>⏳ Estimated time: 20 min</font>

**Exercise 3: Timed acquisition, single channel**

Using the following command you can add the sampling rate and the total number of samples:

``task.timing.cfg_samp_clk_timing(rate=......, samps_per_chan=.......)``

No need anymore for a for loop, so use the code of Exercise 1 and the above command in the code below. 

Measure a signal of 10 Hz for one second. Include a figure with a proper time axis.

> ### <font color='blue'>Hint:</font>
> 1. Make sure you store all measurements without overwriting! That means **fill in inside the brackets for task.read()**

```python
##code for students
sampling_rate = 5000
number_of_samples = ....... # measure one second

with nidaqmx.Task() as task:
    ..................... # task add channel
    ..................... # task timing
    ..................... # measurement

#Plotting
time=.................... # an array depending on number_of_samples and sampling_rate, use np.arange()
plt.plot(time, measurement2, '-*' )
```

```python

```

### Keep this piece of code; you will reuse it in coming exercises!

**Exercise 4: Timed acquisition, multiple channel** <br>
For multiple channels, to be used to for example measure $V_{in}$ and $V_{out}$ at the same time, you need to add two channels. In code that would be:



```python
with nidaqmx.Task() as task:
    
        task.ai_channels.add_ai_voltage_chan("Dev1/ai0")
        task.ai_channels.add_ai_voltage_chan("Dev1/ai1")
```

Your data will be 2 dimensional.<br> Copy plus adapt your previous 1 channel code, and adapt it for two channels. Test it by acquiring the function generators output in one channel, and the SYNC signal in the other. 

```python
# copy +adapt your previous code to 2 channels

plt.plot(time, data_OUTPUT,'b')
plt.plot(time,data_SYNC,'r')
```

```python

```

## SAVE YOUR FILE IN H: DRIVE, OTHERWISE YOU WILL LOSE IT


## EXPLAIN & EVALUATE
> <font color='grey'>⏳ Estimated time: 5 min</font>
* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

* Make sure that you understand all these pieces of code well enough to use and adapt them yourself. You will need to use similar code in coming exercises.

**First discuss within your group, and then with the TA**
1. your evaluation: explain whether and why timed acquisition is better?
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved

```python
#7A analog input
### TO DO="explain whether and why timed acquisition is better?"

### TO DO="2a. abstract"

### TO DO="2b. troubleshooting"

### TO DO="2c. code"

### TO DO="3. what changes would you suggest?"

```

```python
# no recording this time. We totally believe you can do this yourself!
```
