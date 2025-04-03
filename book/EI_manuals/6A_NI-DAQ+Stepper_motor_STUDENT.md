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
- experiment 6A: use NI-DAQ to create digital signals
- experiment 6B: use PicoPI to create a visual Morse signal and observe digital signals
- experiment 6C: program your Alpaca using a mix digital inputs and outputs

Goal: learn how to work with digital signals with NIDAQ and Alpaca

Structure of an experiment:
- Anticipe + Stimulate (15+15+5): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (30+20): with your partner(group of 2)
- Compare + Conclude (15 min): with a group of 4(per table)


# 6A: Using NI-DAQ for digital output
> <font color='blue'>Learning goal:</font> Learn how to use NI-DAQ to create digital signals. Light up led diode and make stepper motor move using NI-DAQ

<!-- #region -->
## BACKGROUND
> <font color='grey'>⏳ Estimated time: 15 min</font>

In this part of the practicals we occupy ourselves with interfaces directly between the real analog world and the PC. Both directions are covered: the conversion of an analog signal outside the PC into a digital representation of the signal inside the PC by an analog-to-digital converter (ADC) and conversely by a digital-to-analog converter (DAC). You can thus directly input an analog voltage to the interface or output an analog voltage from it. The interface is usually executed as a plug-in board with an edge connector that is plugged into an expansion slot of the PC. This type of interface is described with the term data-acquisition board. The DAQ-board is not an independent measuring device; it has no display and no own power supply, and is always used in combination with a PC.

**note: the DAQ board is in the PC. Whenever you want to use the DAQ card through Python/Jupyter notebook (like this assignment) you will need to download the jupyter notebook file on the classroom PC and run it locally.**

Usually the board also has provisions for digital I/O, where the conversion of the digital representation inside the PC to a digital signal outside the PC and vice versa takes place. For this, so-called I/O ports and I/O lines are used. On the board counters and timers are present for the timing of signals.

In this practical we use the NI PCI-6221 DAQ board of National Instruments. You can find this board next to your computer. 

This popular M-series multifunction DAQ board has a. o. the following specifications. Except the counter, you will be using the analog and digital inputs and outputs plus the trigger:
* Analog input: 
    * 16 single-ended or 8 differential channels
    * 16 bits ADC resolution
    * 250 kS/s (kilosamples per second) sampling rate
    * ± 10 V,  ± 5 V, ± 1 V, ± 0.2 V input range
* Analog output:
    * 2 channels
    * 16 bits DAC resolution
    * 740 kS/s per channel update rate
    * ± 10 V output range
* Digital I/O:
    * 24 input/output
* Counter/timers:
    * 2 up/down
    * 32 bits resolution	
    * 80 MHz time base rate
* Triggers:
    * digital


The 68 pins connector on the board is connected to the breakout box BNC-2110.
<table><tr>
<td>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PyDAQ-Next/PyDAQ1-img1.jpg" width=70%></img>
</td>
<td>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PyDAQ-Next/PyDAQ1-img2.png" width=70%></img>
</td>
</tr></table>
<!-- #endregion -->

In this first assignment you will let a motor turn. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PyDAQ-Next/6A_stepper_motor.jpg" width=50%></img>

After powering the motor, the motor can be connected and controlled through the three signal wires: black for ground, and {red and green} for {step and direction}. Note: at this point we do not tell you whether the step is controlled through the red or green wire, this is something you'll have to discover yourself. 
The applied signals are logical (so called TTL) signals, which means digital low 0 = 0v, and digital high 1= 5V. 

- In order to let the motor turn a bit ("make a step"), it needs the signal on the pulse input to go from low to high TTL signal. Whenever the input signal goes from low to high, the motor moves **7 degrees** and the (middle) PULSE LED lights up. The rotation speed is limited to 1 Hz.

- The spinning direction is determined from the left/right input. low signal (0) means clockwise direction, high signal (1) means anti-clockwise direction. The corresponding left or right LED will light up. 



## ANTICIPATE: connection points on the board and signal required to make stepper motor move
> <font color='grey'>⏳ Estimated time: 15 min</font>

1. In order to familiarize yourself with DAQ board, pinpoint on the top BNC connector image where the following connections are. You can make a graphic or write it in text:
* analog in 0 (AI0)
* analog out 0 (AO0)
* Digital I/O line P0.0
* Digital Ground
* PFI 0 

2. Make a prediction (either in text or in drawing) which signals you have to generate in order to:
* let the motor make 2 full turns to the left
* let the motor make 3 full turns to the right
> <font color='blue'>Hint:</font> the 'direction' signal and 'pulse' signal are 2 different inputs on the DAQ!

```python
from ipywidgets import FileUpload
from IPython.display import Image

upload=FileUpload()
upload
```

```python
file_name="6A_1a_daqdescription.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, works on Vocareum if you change the kernel

Image(filename=file_name, width="50%")
```

```python
upload
```

```python
file_name="6A_1b_stepperdescription.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, works on Vocareum if you change the kernel

Image(filename=file_name, width="50%")
```

```python
### TO DO="your answer on which signals you have to generate"

```

## SIMULATE
> <font color='grey'>⏳ Estimated time: 5 min</font>

There is no simulation for this experiment, but do already think about:
1. What would happen if you swap the pulse and left/right signals
2. If these signals are swapped, think about how to adapt. Do you prefer changing wires, or adapting code?

```python
### TO DO="your answers regarding swapping pulse with left/right signal"

```

If you want to have a precap on the lab work, feel free to watch the following movie

```python
# precap
from IPython.lib.display import YouTubeVideo
YouTubeVideo('2oJ4llTsxgc', width = 600, height = 450)

```

## IMPLEMENT & INVESTIGATE 1: Explore the NI-DAQ code for switching on LEDs. 
> <font color='grey'>⏳ Estimated time: 30 min</font>

**You will need to run python locally on Studio Classroom desktop for later exercise.** Note: Run the base installation (no need for conda activate, just jupyter lab), and run it in **H-drive** on the desktop, as this is your personal drive (and the C-drive is reset every few days, which results in lost of files from C). 


In the Studio Classroom (SCR) the board has already been installed and configured with the NI-DAQ driver software and support files. Within NI-DAQ software, the Measurement & Automation Explorer (MAX), has been installed. Feel free to explore this graphical user interface, but note we will only use code from now. In this assignment we start with the digital signals, in later exercises also analog signals will be used. 


### You will need to run python locally on Studio Classroom desktop.

### Switching on LED

* Connect an LED with its long pin into P0.0 and its short pin into D GND of your DAQ breakout box, section Digital and Timing I/O.
You can find the LED in a same box where T-junction.
Note that I/O stands for Input/Output; LED stands for Light Emitting Diode

**Put the LED in the big holes, those make the best connection.** <br>
**Use the orange button to open the clamp (on the bottom of the hole), both when inserting and most importantly during removal**

* Run the code below and make sure the LED is on. If not working: make sure the pins are well-connected and the LED is not broken.

> ### <font color='blue'>Important:</font> 
As you will learn in octal 3 a diode is one-directional. If your LED is not switching on/off, please check whether you connected it correctly

```python
import nidaqmx
import numpy as np
import matplotlib.pyplot as plt
import time
from nidaqmx.constants import AcquisitionType, Edge, LineGrouping

system = nidaqmx.system.System.local()

with nidaqmx.Task() as task:
    task.do_channels.add_do_chan('Dev1/port0/line0')
    task.write(True)

```

```python
### TO DO="Did you get the LED working?"

```

Alter the code below and switch off the LED programmatically after one second.

> <font color='blue'>Hint:</font> The 'with' statement is used for simplificiation with resources. Also see https://www.geeksforgeeks.org/with-statement-in-python/. You can do anything with the task inside the with statement, outside the statement the nidaqmx.Task() is closed and unavailable for further use. 

```python
import nidaqmx
import numpy as np
import matplotlib.pyplot as plt
import time
from nidaqmx.constants import AcquisitionType, Edge, LineGrouping

system = nidaqmx.system.System.local()

with nidaqmx.Task() as task:
    task.do_channels.add_do_chan('Dev1/port0/line0')
    task.write(True)
### TO DO="your code, hint: use time.sleep and task.write"

```

### Now we are going to use 2 LEDs


* Add another LED in P0.1 and D GND.

There are two versions of a code. One contains `line_grouping=LineGrouping.CHAN_FOR_ALL_LINES`, the other one `line_grouping=LineGrouping.CHAN_PER_LINE`.
* Run the codes below. Explore the difference between the two versions. 

```python
import nidaqmx
import numpy as np
import matplotlib.pyplot as plt
import time
from nidaqmx.constants import AcquisitionType, Edge, LineGrouping

system = nidaqmx.system.System.local()

with nidaqmx.Task() as task:
    task.do_channels.add_do_chan('Dev1/port0/line0:1', line_grouping=LineGrouping.CHAN_FOR_ALL_LINES)
    task.write(1),    time.sleep(1)
    task.write(2),    time.sleep(1)
    task.write(3),    time.sleep(1)
    task.write(0),    time.sleep(1)

```

```python
#import nidaqmx
#import numpy as np
#import matplotlib.pyplot as plt
#import time
#from nidaqmx.constants import AcquisitionType, Edge, LineGrouping

#system = nidaqmx.system.System.local()

with nidaqmx.Task() as task:
    task.do_channels.add_do_chan('Dev1/port0/line0:1',line_grouping=LineGrouping.CHAN_PER_LINE)
    task.write([True,False]),    time.sleep(1)
    task.write([False,True]),    time.sleep(1)
    task.write([True,True]),     time.sleep(1)
    task.write([False,False]),   time.sleep(1)
```

Why do we end with `task.write(0)` or `task.write([False,False])`(what do they do)?
> ### <font color='blue'>Hint:</font>

> notice the difference in line_grouping

>comment out lines to see what each individual line does. 

>Or introduce longer pauses, with import time, time.sleep(1) to see which line you are on

Why would you use one code over the other?

**Show to the TA that you got LEDs working!(You can either check it now or keep all assembled till COMPARE&CONCLUDE and check off at the end, or take a video proof)**

```python

### TO DO ="Explain which LineGrouping you choose" 

```

## IMPLEMENT & INVESTIGATE 2: Make stepper motor move
> <font color='grey'>⏳ Estimated time: 20 min</font>


Now you are finally ready to make stepper motor move! Take the stepper motor from the caddy
and power it. Connect it to DAQ:
* The black wire goes to D GND
* The red and green are for left/right and making a step (for you to figure out which one is which )). Connect them to P0.0 and P0.1 (figure out which cable where).

## When you want to remove the cables from the big holes, press a orange button (with a pen tip or a screwdriver) next to them to release. DO NOT PULL when you encounter resistance

Adapt the code below to let the motor spin first 2 full turns to the left and then 3 full turns to the right.
>  <font color='blue'>Hint:</font>
First make it spin just to the left, it will help you figure out if you connected the cables right

> '??' indicates you should add a line of code or complete preexisting code there


```python
#if you want to run just part of the code, comment the rest out(don't delete it!)
with nidaqmx.Task() as task:
    task.do_channels.add_do_chan('Dev1/port0/line0:1',line_grouping=LineGrouping.??   ) 
    left_right=?? # are you going left or right
    time_to_wait=??                                                                               
    for ii in range(       ??    ): # you calculated that in your anticipate!
        task.write(  ??  ),    time.sleep(time_to_wait) # make it move! (this and next line) get inspired by code in I&I1
        #??
    #??   switch direction
    for ii in range(    ??        ):
        task.write(    ??    ),    time.sleep(time_to_wait)  
        #??
    #?? (end 'with' statement) 
```

```python
### TO DO="# did you power the motor?"

```

If you need more ideas, here are a couple more hints for you!
> ### <font color='blue'>Hint:</font>
> - You have used task.write() before, to light two LEDs. Use that code as inspiration. 
> - Try to figure out which output port corresponds to which wire. You can use NI-MAX.
> - If you have them connected wrongly, think about solving it either with the wires or the code (whatever you find easier)
> - Expected: a functional code that makes the motor first spin 2 full tunrs to the left and then 3 full turns to the right.


### Congratulations! You made stepper motor move. You will need to show to the TA that it is working either in person or take video proof

### Remember to save this notebook in H drive on the desktop! We also recommend uploading it to vocareum right away, so if you forget to save it in H drive/something goes wrong, you still have access to this notebook

<!-- #region -->
## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**
1. Show your motor move in the requested way
2. Reflect on the journey from predicted signal to functioning Python code: which steps did you take
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
#6A stepper motor
### TO DO="1. show your TA your spinning motor"

### TO DO="2.reflect on your journey from prediction to correct motor movement"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python
#no recording this time: just look at your motor spinning 
```
