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

<!-- #region -->
Experiments of this week:
- Experiment 4A: Work with fourier transforms on the oscilloscope
- Experiment 4B: Implement an RC circuit with PicoPi
- Experiment 4C: Investigate RCCR circuit with PicoPi and compare to an RLC circuit in LTSpice 


Goal: Work with various types of filters and understand their properties 

Structure of an experiment:
- Anticipate + Simulate (55 min): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (20+40 min): with your partner(group of 2)
- Compare + Conclude (15 min): with a group of 4(per table)

<!-- #endregion -->

<!-- #region -->
# 4C: RCCR filter
(Investigate an RCCR circuit with PicoPi and compare to an RLC circuit in LTSpice)


> <font color='blue'>Learning goal:</font> Understand the properties of an RCCR and RLC circuit. 

##  BACKGROUND:
> <font color='grey'>⏳ Estimated time: 15 min</font>

In this assignment you will predict and measure the behavior of an RCCR circuit, simulate an RLC circuit, and will compare whether they quantitatively behave the same. These are combined filters, and therefore instead of looking at a single high/low pass filter, you will now have to look at the combined behavior. 

When combining two low pass filters, the lowest -3dB point will be having most influence on the overall characteristics of the filter. Similar, when combining to high pass filters, the higherst -3dB point will have have most influence. 

The most interesting is when combining a low with a high pass filter. Depending on whether the lows pass -3dB is lower, equal or higher than the high pass -3dB, you can get different bandwidths and different max amplitudes. Visually you could see this as overlaying the two filters, and the overall filter is the area between the two filter graphs. 

<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/4C_bandpass_combined_filters.jpg" width="700"/>
</div>

You can calculate characteristics of a bandpass filter, in a similar way as for a single pass filter. Just derive $\frac{V_{out}}{V_{in}}$ and look at extreme cases for $\omega$. 
When measuring the characteristics in the lab, for a single pass filter with sine input you can find the -3dB frequency as the frequency at which Vout= Vin/sqrt(2). For a bandpass filter you will have to locate the resonance frequency (with highest amplitude), which is flanked by two -3dB points; the difference between the two -3dB frequencies is the bandwidth. 




Watch the video below on the derivation of the properties of an RLC filter: 
<!-- #endregion -->

```python
# RLC circuit  

from IPython.lib.display import YouTubeVideo
YouTubeVideo('H40Er_Djwc0', width = 600, height = 450)
```

## ANTICIPATE: RC + RC behaviour
> <font color='grey'>⏳ Estimated time: 15 min</font>

Below you see a circuit which combines two RC filters, one with 22 kOhm, the other with 3.3 kOhm. 
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/picopi4_4_RCC_circ.jpg" width="700"/>
</div>

You can see this circuit as two RC filters in sequence.
* Derive their filter type (low/high/bandpass). 
* Calculate the frequency of -3dB point(s).
* Predict what the effect of these two parts in combination will be on the output signal.


```python
###TO DO="your answer to the above 3 questions"

```

in order to get a quick start on the simulation, feel free to watch the precap video of 2022

```python
## precap LTSpice RLC

from IPython.lib.display import YouTubeVideo
YouTubeVideo('eRc66EzcYe0', width = 600, height = 450)

```

## SIMULATE 1: RLC circuit behaviour 
> <font color='grey'>⏳ Estimated time: 15 min</font>

Build the below circuit in LTSpice, with R=3kΩ , L=5mH , C=7nF. Evaluate the effect of this filter.
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS4_RLC_circuit.jpg" width=50%></img>





__Exercise 1:__ Find the bandwidth of the circuit. 

* What is the value of (max) Vout you look for at -3dB?
* Do you expect the -3dB points to be symmetric around the resonance frequency?
* Change the input frequency to find the -3 dB points (accuracy 100 Ohm) by trial and error.
* Also have a look at the phase at these -3 dB points!
> __<font color='blue'>Hint:</font>__
When you have found the -3dB points, try to plot both Vout at one of the -3dB points (change the frequency) and Vin in a single graph. Using the time delay between two peaks and the period (which corresponds to 360 degrees), you can determine the phase delay.


**Support your findings with a screendump of the simulation**


```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python

file_name="4C_1_bandwidthRLC.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        f.write(upload.data[-1])
Image(filename=file_name, width="50%")

```

```python
### TO DO="your bandwidth, discuss symmetry of the band, and phase of the signal"

```

### <font color='red'>Optional challenge</font>
Try to retrieve the BW from Vout (in time) with Rser=100 Ohm. Have a look at the Vout in time for Rser=100. The amplitude of Vout should decrease with exp(-i t BW/2). 
* Derive the BW from Vout for Rser=100 Ohm?
* why was this approach impossible for Rser=3kOhm?

```python
### TO DO="your answer (optional)"
```

```python

```

## SIMULATE 2: RCCR 
> <font color='grey'>⏳ Estimated time: 10 min</font>

Use the file "4_RC_CR_RCCR_Simulate" that you can find in the folder for this week. It will contain an RCCR circuit as shown here: 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/4C_RCCR_Simulate.png" width=50%></img>

Run this simulation as you did in 4B to find the -3dB points. 
* Do they match the predicted values? 
* How do you think it will affecr the amplitude of Vout?
* If you implement this circuit on the Alpaca do you expect to find similar values? 


```python
### TO DO="your answers on the above 3 questions"

```

in order to get a visual introduction to the measurements with the picopi, feel free to watch the following precap movie from 2022

```python
#precap
from IPython.lib.display import YouTubeVideo
YouTubeVideo('hpfYnre2s2g', width = 600, height = 450)
```

<!-- #region -->
## IMPLEMENT & INVESTIGATE 1:  Analog Input & Output 
> <font color='grey'>⏳ Estimated time: 20 min</font>

In assignment 4B you already used the analog `AinX - ext` pins on an area of the Cria called the **Multifunction connector**. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi1_1_analog in.jpg" width=50% style="float:right"></img>

### DAQ
The code in the cell below allows you to output a continuous, arbitrary voltage between 0 and 3.3V from the ALPACA using the Digital-to-Analog Converter (DAC). Interfacing with the DAC is done using the `functiongenerator` module, and made to imitate the function generator in the Student Class Room as much as possible. From the `functiongenerator` module you import `FuncGen`, which you can use in parallel to other operations using the `with` environment. See the example below:

```python
from functiongenerator import FuncGen, Sine

print("Starting function generator")

with FuncGen(Sine(Vpp=2, offset=1, freq=100)): # Use the function generator
    print("Doing something else in the mean time") # Do something else in parallel
    time.sleep(10) # Wait 10 s. After this the function generator turns of automatically
     
```
Similar to the limitation of the digital channel, the analog input channel is also **restricted to 0-3V**. So be careful with connecting signals. In this assignment you will output a sine from the Analog output channel and read it in into Analog in, plus connect it to the Voltmeter of the Cria. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi1_5_DAC_A.jpg" width=50% style="float:right"></img>

<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>
    
In the simple example below, the function generator is turned on while the PicoPi waits for whatever is inside the `with` to finish (acts as a while loop). Because the stuff on the inside is a delay, the end result is that the function generator is on for 5 seconds
    
* Connect *DAC-A* output (on ALPACA) to the voltmeter (on the multifunction pin)
* Run the code
<!-- #endregion -->

```python
%serialconnect to --port="COM4" 
```

```python
from functiongenerator import FuncGen, DC
import time

with FuncGen(DC(V=2)):
    print("Procedure starts - Duration: 5 seconds\n")
    print("Have a look at the Voltmeter LEDs")
    time.sleep(5)
    print("\nProcedure ended")
```

<!-- #region -->
<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>

Now we change to an AC signal, specifically to a Sine with a certain frequency, $V_{PP}$, and offset. 

For example the following function generates a sine with amplitude 1.5V and DC value 1.5V. 
```python
with FuncGen(Sine(Vpp=3, offset=1.5, freq=10)):
```

* Connect *DAC-A* to *Ain0* (top-left pin Multifunction connector). 
* Run the code
    
> <font size=6>ℹ️</font>
>
> <font color='00a6ed'> You want to use the **breadboard** to connect the output of *DAC A* to both the voltmeter and *Ain0*. Or run the code once with the Voltmeter connected, and the second time (after checking it is save) Ain0 connected.
    <font>
<!-- #endregion -->

```python
from functiongenerator import FuncGen, Sine
from machine import ADC
import time
import numpy as np
import matplotlib.pyplot as plt


# Initialize the output pin: Ain0 has index 26, Ain1=27, Ain2=28.
a0 = ADC(26) 

# Set the frequency to 1Hz
frequency = 1
# Initialize an array for the wave that will be plotted
waveform = np.zeros(300)
time_steps = np.zeros(300)

# Set the sampling rate to 5ms (0.005s)
sampling_rate = 0.005

with FuncGen(Sine(Vpp=3, offset=1.5, freq=frequency)):
    start_time = time.ticks_ms()
    for ii in range(300):
        integer = a0.read_u16()
        
        current_time = time.ticks_ms()
        elapsed_time = current_time - start_time # Calculate the difference from the start time
        time_steps[ii] = elapsed_time # Append the elapsed time to the time_steps list
        
        voltage = (integer / 65535) * 3.3
        waveform[ii] = voltage  # Stores each measurement of the voltage

        
        time.sleep(sampling_rate)  # Time step
            
#time_steps = np.arange(len(waveform), dtype=np.float)

print(f"Waveform:\n{waveform}\n")
print(f"Time Steps:\n{time_steps}\n")
print(f"Waveform Shape: {waveform.shape}")
print(f"Time Steps Shape: {time_steps.shape}")
```

```python
plt.plot(time_steps, waveform, label='Frequency: {:.2f} Hz'.format(frequency)) #This plots for last frequency
plt.xlabel('Time [ms]')
plt.ylabel('Voltage [V]')
plt.legend()
```

```python
# Initialize an array for the wave that will be plotted
waveform = np.zeros((2, 300))  # Use a 2D array to store waveforms for each frequency
# We use 2 frequencies in the range 1-10Hz 
freq_range = np.linspace(1, 10, 2) 
time_steps = np.zeros((2, 300))  # Use a 2D array to store time steps for each frequency
# Try to understand the for loop over the frequencies 
for idx, frequency in enumerate(freq_range):  # Use enumerate to get the index

    with FuncGen(Sine(Vpp=3, offset=1.5, freq=frequency)):
        start_time = time.ticks_ms()
        for ii in range(300):
            integer = a0.read_u16()
            current_time = time.ticks_ms() 
            elapsed_time = current_time - start_time    # Calculate the difference from the start time
            time_steps[idx, ii] = elapsed_time  # Store the elapsed time in the 2D array
            
            voltage = (integer / 65535) * 3.3
            waveform[idx, ii] = voltage  # Store each measurement of the voltage in the 2D array
            
            time.sleep(0.005)  # Set the sampling rate

print(f"Waveform:\n{waveform}\n")
print(f"Time Steps:\n{time_steps}\n")
print(f"Waveform Shape: {waveform.shape}")
print(f"Time Steps Shape: {time_steps.shape}")
```

```python
plt.plot(time_steps[0,:], waveform[0,:], label='Frequency: {:.2f} Hz'.format(freq_range[0])) #This plots for last frequency
plt.plot(time_steps[1,:], waveform[1,:], label='Frequency: {:.2f} Hz'.format(freq_range[1])) #This plots for last frequency
plt.xlabel('Time [ms]')
plt.ylabel('Voltage [V]')
plt.legend()
```

<!-- #region slideshow={"slide_type": ""} -->
Make sure you understand the concept of looping over all frequencies. This makes it a lot easier for you to compare multiple frequencies and you do not need to change them manually. In it especially useful when you want to plot the results live.
<!-- #endregion -->

<!-- #region -->
## IMPLEMENT & INVESTIGATE 2: RCCR (or CRRC) circuit 
> <font color='grey'>⏳ Estimated time: 40 min</font>

**Disconnect the wires from the analog inputs (Ain0,Ain1,Ain2) on the Cria for safety reasons**


Adapt your circuit on your Alpaca so that it consists of both filters: 
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi4_4_RCC_circ.jpg" width="700"/>
</div>

**Caution**: Applying a too high voltage (or negative voltage) to the Cria may destroys the chip on the PicoPi (=delay for you). Thus, to ensure that the range of the signal is within safe limits, we are going to use the Voltmeter on the Cria before connecting the signal to your ALPACA. It is good practice to always use the Voltmeter before connecting any signal to the Cria and PicoPi, in order to keep your Cria out of harm's way. 

    
<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>
 <div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi4_3_jumper_pos_no_pin_2nano.jpg" style="float:right" width="400"/>
</div>
    
* Build the circuit as shown in the image below 
* Connect the circuits Vout to the voltmeter
* Connect both USB cables to the Alpaca, and turn the switches for the +12 and -12 voltage supply to the ON position
* Make sure you have no jumpers on *AMPLIFIER DIRECT TO NANO*, remove the one indicated in the blue circle in photo on the right:

* Run the code below, which generates a sine wave, and check the LEDs of the voltage meter


    
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/f20061156f3e13d96652af0e301d7d35e08ebe5a/PicoPI/picopi4_6_RCC_build.png" width="70%"/>
   

<!-- #endregion -->

```python
%serialconnect to --port="COM4" 
#ADD COM PORT ABOVE, e.g. --port="COM3"
```

<!-- #region -->
If the measurement and setup was correct, you should have seen that a negative and a positive voltage is generated at the ouput of your circuit. **This negative voltage can damage your pico.** <br>
Therefore we need to use *AMP 0* or *1* to measure our signal. This **amplifier** can be used as a **protective circuit**, which **forces your signal to be between 0 and 5V**, which should not damage the picopi. Make sure to use the amplifier that is present on the ALPACA, to measure the signal when you know it's going to have voltages exceding the PicoPi's limits. This way your PicoPi is always protected. 

This will be implemented below: 

<font color='ff822d' size=6> 📝 <font> <font color='ff822d' size=4> **Todo**: <font>
* Build the circuit as shown in the image below
* Connect the output of the DAC, e.g. $V_{in}$, to _Ain0_ on the multifunction connector.
* Connect the output of your circuit, e.g. $V_{out}$, to the _SIGNAL+_ pin at _AMP1_ (on the ALPACA)
* Connect the jumpers as indicated using magenta in the image below. You do need one jumper for _AMPLIFIER DIRECT TO NANO_ (I suggest you use a wire as removing it will be easier in the future).
* Similar as before, a sine wave will be used. The code used to generate this sine wave and to perform the measurement is provided below. The frequency of the signal can be set in the code.


<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/f20061156f3e13d96652af0e301d7d35e08ebe5a/PicoPI/picopi4_7_RCC_build.png" width="70%"/>


<!-- #endregion -->

```python
from machine import ADC, Pin
import time

import matplotlib.pyplot as plt
import numpy as np

from functiongenerator import FuncGen, Sine

MEASUREMENTS = 300 # number of measurements

v_in = np.zeros(MEASUREMENTS, dtype=np.uint16) # Store ADC output as integer, convert to voltage later
v_out = np.zeros(MEASUREMENTS, dtype=np.uint16)
tt= np.zeros(MEASUREMENTS)
start = time.ticks_us() #Measure in us 

Ain0 = ADC(26) # Pin 26
Ain1 = ADC(27) # Pin 27

with FuncGen(Sine(Vpp=3, offset=1.5, freq=110)):
    time.sleep(0.5) # Wait half a second for the Function Generator, before continuing to measure

    for ii in range(MEASUREMENTS):
        v_in[ii] = Ain0.read_u16()
        v_out[ii] = Ain1.read_u16()
        tt[ii] = time.ticks_us() #
       # tt[ii]=ii
        time.sleep_ms(1) # Sleep in milliseconds
tt=(tt-tt[0])*1e-3     
#print(v_in)
#print(v_out)
    
plt.plot(tt,v_in * 3.3 / 65535, color='r', label='DAC A (Ain0) [V]') # Convert integer to voltage
plt.plot(tt,v_out * 3.3 / 65535, color='b', label='Vout (Ain1 via AMP1) [V]')
plt.ylabel('Signal [V]')
plt.xlabel('Time [ms]')
plt.legend()
```

```python
from functiongenerator import FuncGen, Sine
import time

with FuncGen(Sine(Vpp=3, offset=1.5, freq=10)):
    time.sleep(10)


### TO DO ="your answer, what does the voltmeter show"


```

You should notice the shape of Vout and the amplitude of Vout differ from Vin. 

__Exercise 1:__ Explain why there is a difference between Vin and Vout, and if this is what you expected for this circuit.
> __<font color='blue'>Hint 1:</font>__ Do you see the full sine? Is the signal shifted vertically? If you have trouble seeing the sine, try changing frequency to 50Hz 

> __<font color='blue'>Hint 2:</font>__ DC components have 0Hz, what do you think happens when you pass them through the bandpass filter?

```python
### TO DO="your answer, explaining the difference between Vin and Vout"

```

Because of one of the RC filters, the average of the output signal is now zero (make sure to understand why this is - Hint2 above). You can also observe the lack of negative voltage. This is thanks to the amplifier.

Despite only a part of the signal being measured, the 3 dB points can still be determined from the plot.

__Exercise 2:__ Set the frequency back of 110 Hz in the code, and calculate the amplitude (the max of the positive part of the sine wave) of the measured signal. 
* Report the amplitude. Why do you think it is not half of the Vin amplitude? Think back to ANTICIPATE and SIMULATE 2

```python
print(np.max(v_out*3.3/65535)) ## you already did that measurement, just extract the max voltage


### TO DO="your found peak voltage"



```

Now you found the maximum voltage value. Next, you will compare the -3dB points to the one you found in ANTICIPATE and SIMULATE 2. Rememeber that bandpass filter has 2 -3dB points, as the strength of the signal fall down in low frequencies (thanks to high pass filter component) and high frequencies (thanks to low pass filter component). Think back what -3dB means in terms of voltage drop. <br>
__Exercise 3:__ Calculate the Voltage you should see at -3dB points based on vmax you found

```python
### TO DO="Which voltage you should see at -3dB - V(-3dB)"

```

__Exercise 4:__ Lower the frequency by changing the parameter called "freq" in the code. Look for the lower -3 dB point of the signal by first setting the predicted frequency (ANTICIPATE) and then varying it to find the V(-3dB) voltage
* You can reuse/rerun the above code, the three cells which do the acquisition, fetch the file and extract the max value
* Does the frequency match the predicted value? 

> __<font color='blue'>Hint:</font>__ Did your SIMULATE 2 match ANTICIPATE? Think about why was that and apply the same logic to Alpaca

```python

### TO DO="your answer for the lower -3dB point"

```

Because the above system is a band pass filter, we not only have a lower cut off frequency but also a higher limit for the frequency.

__Exercise 5:__ Increase the frequency to look for the upper 3 dB point, up to a max of 500 Hz(physical limitation of Alpaca). You might want to change the parameter num_samples to 100 to better visualize the result.
* At what frequency is the upper 3 dB point expected - look ANTICIPATE? 
* Set that frequency and measure the peak voltage at this expected -3dB frequency
* Compare the measured cut off frequency with the theoretical value, does this result surprise you? (it should)
* find the  V(-3dB) voltage by varying the frequency and report where it is

```python

### TO DO="your higher -3 dB point"


```

Compared to the equipment in the studio classroom, you see that measuring with the Alpaca, has its limitations. In this case on of these limitations is that the frequency of the sine wave is limited to around 500 Hz. Designing a system with cut off frequencies which are further apart is in this case not possible, but would be possible with the equipment located in the studio classroom.

<font color='red'>__Optional challenge 1__:</font> Switch position of the resistors<br>
When we switch only the resistors around we get a circuit, which behaves differently.
* Will the output signal of the system be the same, or will it respond differently when we switch around the resistors? (think about Vmax, -3dB points)
* Argue what happens for a frequency of 20, 280 and 150 Hz.


```python
### TO DO="answers optional challenge"

```

<font color='red'>__Optional challenge 2__:</font> In the code that you used above, can you add a for loop over the frequencies to automatically find the -3dB points without having to manually change it?

```python
### TO DO="your code idea (there is a solution at the bottom of the assigment )"


```

## COMPARE & CONCLUDE:
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait until all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:** 
1. Why does the output only show half of the sine wave with the RCCR circuit
2. How well did your predictions agree with the measurements on the -3dB frequencies for the RCCR circuit? Explain the difference.
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved

```python
### TO DO =" 1. why do you see only half of the sine with the RCCR circuit?"


### TO DO="2. give values and explain the difference between found measured and predicted -3dB frequencies"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"
```

> <font color='red'> Optional code:</font>
Below is an adapted version of the code you used above and completely optional to do. The concepts used here will be explained in the next octal. As you can see there is a loop over a range of different frequencies (beware, the code takes a long time to run, thus, maybe you have some smart ideas to improve this code):

```python
from machine import ADC, Pin
import time

import matplotlib.pyplot as plt
import numpy as np

from functiongenerator import FuncGen, Square

MEASUREMENTS = 300 # number of measurements

v_in = np.zeros(MEASUREMENTS, dtype=np.uint16) # Store ADC output as integer, convert to voltage later
v_out = np.zeros(MEASUREMENTS, dtype=np.uint16)
tt= np.zeros(MEASUREMENTS)
start = time.ticks_us() #Measure in us 

Ain0 = ADC(26) # Pin 26
Ain1 = ADC(27) # Pin 27

FREQ=np.arange(1,500,1)
VOUT=np.zeros(len(FREQ))
for ff in range(len(FREQ)):
    with FuncGen(Sine(Vpp=3, offset=1.5, freq=FREQ[ff])):
        time.sleep(0.5) # Wait half a second for the Function Generator, before continuing to measure
        if ff%10==0: print(FREQ[ff])
        for ii in range(MEASUREMENTS):
            v_in[ii] = Ain0.read_u16()
            v_out[ii] = Ain1.read_u16()
            tt[ii] = time.ticks_us() #
           # tt[ii]=ii
            time.sleep_ms(1) # Sleep in milliseconds
    tt=(tt-tt[0])*1e-6     
    
    VOUT[ff]=np.max(v_out)
    


```

```python
plt.plot(FREQ, VOUT * 3.3 / 65535, color='b', label='Output voltage')
plt.ylabel('Signal [V]')
plt.xlabel('freq[Hz]')
plt.legend()
```

```python
### TO DO='perhaps you can make a smarter frequency range' 
A=(2**np.arange(0,9,0.2))
print(len(A),A[0],A[-1])

A=np.arange(1,500,1)
print(len(A),A[0],A[-1])

```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip


```python
## recording LTSpice RLC simulation

from IPython.lib.display import YouTubeVideo
YouTubeVideo('LqNr2Sj7V6E', width = 600, height = 450)
```

```python
## recording picopi measurements

from IPython.lib.display import YouTubeVideo
YouTubeVideo('oQ3e1UPY-B0', width = 600, height = 450)
```
