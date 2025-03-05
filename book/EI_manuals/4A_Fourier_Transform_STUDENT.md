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

<!-- #region -->
Experiments of this week:
- Experiment 4A: Work with fourier transforms on the oscilloscope
- Experiment 4B: Implement an RC circuit with PicoPi
- Experiment 4C: Investigate RCCR circuit with PicoPi and compare to an RLC circuit in LTSpice 


Goal: Work with various types of filters and understand their properties 

Structure of an experiment:
- Predict + Simulate (10+10+10): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Build + Measure (20+20+10): with your partner(group of 2)
- Evaluate (15): with a group of 4(per table)


<!-- #endregion -->

# Experiment 4A: Scope Spectrum
(Work with Fourier Tansforms using the oscilloscope)
><font color='blue'>Learning goal:</font> To be able to visualize the Fourier transform on the oscilloscope and relate changes in the frequency spectrum to changes in your signal.

Materials used:
* Function generator 
* Oscilloscope

## BACKGROUND
> <font color='grey'>⏳ Estimated time: 10 min</font>

So far we have visualized our signals only in the time domain (amplitude vs. time). Using the oscilloscope, however, we can also visualize signals in the frequency or spectral domain (amplitude vs. frequency). The process of going from time to spectral domain is called a Fourier transform. The details will be explained later in much more detail during the course Signals and Systems. Here, we will limit to observe the spectral domain of different waveforms in a qualitative way. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/4A_Sineft.png" width=70%></img>

A sinusoid is characterized by a single (or so-called fundamental) frequency, and can be seen as the sum of two exponentials: sin⁡(ωt)=(exp⁡(iωt)-exp⁡(-iωt))/2i . It’s spectrum will show two peaks at ±ω.
Any signal can be written as sum of sines (or exponentials). You could imagine that if you add higher harmonics (multiples of the initial freuqencies), you can alter the shape of the signal from sine for example into square
(https://en.wikipedia.org/wiki/Square_wave):
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/Fourier_series_for_square_wave.gif" width=30%></img>

A block wave contains by definition only odd harmonics ($f_0, 3f_0, 5f_0$, ....). A triangular wave also contains higher harmonics, but with a lower amplitudes than the block wave. You could therefore change the shape of a signal, by altering the amplitude of the higher harmonics. 




Watch the following video to familiarize yourself with the subject, run the code to access the video:

```python
## Fourier Transform -precap

from IPython.lib.display import YouTubeVideo
YouTubeVideo('1X9d8mmABXk', width = 600, height = 450)

```

## ANTICIPATE
> <font color='grey'>⏳ Estimated time: 15 min</font>

In the practical session you will work with a sine wave and a block wave. 
* Draw the fourier transforms of a sine and block (square) wave in a graph
* Explain which circuit you can use to change this block wave into a triangular wave without altering the settings of the function generator?

> **<font color='blue'>__Hint:__</font>**
The triangular wave and the block wave contain similar higher frequencies. The amplitudes of the triangular higher frequencies are less than the block wave ones. You should have calculated a cut-off frequency blocking the higher harmonics using the capacitors and resistors you have available on your testboards.


```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload

```

```python
file_name="4A_1_drawings.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

```python
### TO DO ="which circuit can you use to change a block wave into a triangular wave?"

```

## SIMULATE
> <font color='grey'>⏳ Estimated time: 10 min</font>

Simulate the circuit to make a block wave into a triangular wave. You can pick your own input frequency and components values. Upload the screenshot of your circuit and the graph
* Which components do you use?
* Explain whether the amplitude of Vout and Vin are equal?

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload
```

```python
file_name="4A_2_triangle_sim.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

```python
### TO DO ="describe the outcome of your simulation, which components and which Vout&Vin?"

```

## IMPLEMENT & INVESTIGATE 1: Visualize the fast Fourier transform (FFT) of a sine on the scope
> <font color='grey'>⏳ Estimated time: 20 min</font>

The Fourier transform states that a signal or waveform can be represented as a sum of sine waves. 
To observe this on the oscilloscope:
* Acquire a sine wave of 50Hz and 2Vpp. 
* Visualize this signal in the frequency spectrum by using the FFT function of the Math key. 
* Select Display/Split for better visualization.
* Select Scale ->Vrms
* Make sure you see the full fourier transform - on the second page of the menu you can adjust the resolution

Remember that the spectrum of the sine wave shows two peaks at ±ω. On the scope however you only see the positive spectral axis, so only one peak is shown. 




Vary the frequency of the sinusoidal wave. What happens to the peak in the frequency spectrum? 

Now change the amplitude of the signal. Observe the changes. You can use the cursor controls to move along the x and y axes. In the menu CURSORS select mode MANUAL, SOURCE FFT, and TYPE voltage or time to switch between horizontal and vertical cursors. 


> **<font color='blue'>__Hint:__</font>**
The result of a Fourier transform is a graph with frequency on the x-axis, while the amplitude is represented by the y-axis. The changes you have observed should match this description.

```python
### TO DO="your answer: for higher frequency the peak ..... for lower amplitude the peak ......."

```

<!-- #region -->
## IMPLEMENT & INVESTIGATE 2: FFT of a block wave
> <font color='grey'>⏳ Estimated time: 10 min</font>


Generate the following block or square wave (scales are 1V/div and 200 us/div)- match it exactly using those settings. You might want to switch off Math to see the signal on the fullscreen.
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC7%20block%20wave.jpg" width=30%></img>

* Set up the oscilloscope to perform a FFT on the waveform (show both FFT and the waveform in the same screen). <br> You  find this function under math.

* Spread out the FFT horizontally until you can distinguish the fundamental frequency and harmonics.
    * What is the fundamental frequency of this signal? 
    * What are the frequencies of the following peaks? 
    * What is the amplitude of these peaks compared to the amplitude in the fundamental frequency?



<!-- #endregion -->

```python
## your answers:

# upload scope picture with Vin=square
# upload a schematic of the circuit

### TO DO=" fundamental frequency, frequencies of first 3 peaks, plus trend amplitudes"

```

Upload an image of the scope.

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload

```

```python

file_name="4A_3_scope_screen.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

## IMPLEMENT & INVESTIGATE 3: turn a block wave into a triangular wave
> <font color='grey'>⏳ Estimated time: 20 min</font>

You have previously anticipated(in ANTICIPATE) a circuit that is able to change this blockwave into a triangular wave. Build the circuit with testboard 1 components and measure+record the outcome.

**Remember**, you are **not** supposed to **change the settings**, except the output load on the function generator.
> **<font color='blue'>__Hint:__</font>**
Use the variable resistor (knob) to get the resistance you need!


Upload an photo of the circuit and the scope with Vout=triangle 

```python
from ipywidgets import FileUpload
from IPython.display import Image
import os

upload=FileUpload()
upload
```

```python

file_name="4A_4_circuit.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

```python
upload
```

```python

file_name="4A_5_scope_triangle.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")

```

```python
### TO DO="describe which parts you used to make a triangular wave out of a block wave"

```

## COMPARE & CONCLUDE:
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait until all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:** 
1. How does the position and height of a FFT of as sine changes with varying frequency?
2. How did you succesfully filtered the block wave into a triangular wave? 
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved

```python
#4A Fourier transform

### TO DO ="1. How does the position and height of a FFT of as sine changes with varying frequency?"

### TO DO ="2. How did you succesfully filtered the block wave into a triangular wave? "

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```


If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip

```python
## recording

from IPython.lib.display import YouTubeVideo
YouTubeVideo('rG1sAAUFC0g', width = 600, height = 450)

```

```python

```
