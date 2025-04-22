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
- Experiment 9A: perform Anti-Aliasing using analog filter
- Experiment 9B: use Digital Filtering and Averaging to improve signal quality
- Experiment 9C: apply Signal Denoising by phase shifiting & averaging

Goal: Explore three types of filters (anti-aliasing, digital filtering and averaging) and determine which type of filter is best for which signal

Structure of an experiment:
- Background+ Anticipate + Simulate (5+15+0 min) : per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (40 min): with your partner(group of 2)
- Compare + Conclude (10 min): with a group of 4(per table)



# 9A: Anti-Aliasing 

> <font color='blue'>Learning goals:</font> Explore how to effectively deal with aliasing

## BACKGROUND
> <font color='grey'>⏳ Estimated time: 5 min</font>

Aliasing is undesired for interpreting spectra of sampled signals. Since aliasing occurs during acquisition, one cannot distinguish correctly sampled and the aliased frequencies from an already acquired signal.

How to avoid aliasing:
* increase the Nyquist frequency 
* remove future aliased frequencies before sampling by the ADC
* use prior knowledge of the expected sample to filter digitally 

In this assignment, we will implement an analog anti-aliasing filter. 


## ANTICIPATE: which filter for anti-aliasing and which effect?
> <font color='grey'>⏳ Estimated time: 15 min</font>

To visualise the effect of anti-aliasing, we will have a look at a square wave signal, acquired at a conveniently chosen sample rate (such that higher aliased harmonics do not overlap with the original signal). From Analysis you should know how the spectrum of a square wave looks. It is also in your formula sheet. You might want to recap that if yoou have troubles completing this exercise.

**Exercise 1** recap block wave spectrum

* For a block wave of 3kHz, draw the frequency spectrum including the first five higher harmonics. Pay attention to the location of the harmonics, plus the general trend in amplitude. 
* Assume that you measure with a Nyquist frequency of 16.5 kHz. Redraw the spectrum, and pay attention to where the aliased peaks above Nyquist end up.
* Explain which type of filter, with which -3dB frequency, you should use to reduce the amplitude of the aliased frequencies only (so you are left with non-aliased frequencies). 

> ### <font color='blue'>Hint:</font>
> * do you remember which harmonic frequencies occur for a blockwave? Which frequency does the 5th harmonic has?
> * what is the relation between Nyquist and sampling frequency? Which one is the frequency you set for the acquisition?


```python
### TO DO="your predictions for the first five harmonics, plus which frequencies are measured after aliasing"

```

```python

from ipywidgets import FileUpload
from IPython.display import Image
import os
upload=FileUpload()
upload

```

```python
# your spectrum
file_name="9A_1_spectra.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%")
```

## SIMULATE
> <font color='grey'>⏳ Estimated time: 0 min</font>

No simulate part for this assignment, but feel free to watch the precap movie

```python
#precap
from IPython.lib.display import YouTubeVideo
YouTubeVideo('IajSgvQN9Po', width = 600, height = 450)
```

## IMPLEMENT & INVESTIGATE 1: anti-aliasing filter measured
> <font color='grey'>⏳ Estimated time: 40 min</font>

**Exercise 1.1**: Copy-paste your code for `calculate_fft` here.

```python
### TO DO="copy your code for calculate_fft here"


```


<!-- #region -->
**Exercise 1.2**: Do a measurement in which you acquire a signal transmitted through a (low pass) filter, as well as the unfiltered signal (taken directly from the function generator). 
* The filter is in the caddy (looks like an aluminium box with IN and OUT coax connector) 
* Use a Nyquist frequency of 16.5 kHz. , compare the unfiltered and filtered signal
* set the function generator to a square wave of 3 kHz
> <font color='blue'>Hint:</font> you know what the Nyquist frequency is, so you should have no problem to figure out the sampling rate


After taking the acquisition compare the two signals, and answer the following questions:
* Which signal frequencies have a different amplitude? 
* Which amplitudes are higher (unfiltered or filtered)?
* Which frequencies are aliased?
* Explain how a low pass filter can help against aliasing. 

<!-- #endregion -->

```python
## your code, based on Analog Input DAQ and fourier transform

#
#put number of samples to 110 - so you measure 1 sec at least, you can try a bigger number, but be aware that it will lengthen
# the time it takes the code to run

# don't forget to add two Analog in channels, check the np.shape of your data afterwards
# task.ai_channels.add_ai_voltage_chan("Dev1/ai0")
# task.ai_channels.add_ai_voltage_chan("Dev1/ai1")
# save the plots at the end

# with Pillow you can automatically save images: plt.savefig('image'+str(set_freq)+'.jpg')
# if receiving an error, do pip install Pillow in an Anaconda command window
```

```python
### TO DO="your code to measure a single signal with nidaqmx"

```

```python
### TO DO ="your answer to the reflecting questions, how the analog filter acts against aliasing"
```

<!-- #region -->
## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


Were your predictions correct? If not, discuss how to improve

**to check off by a TA:**
1. Discuss when aliasing occurs (before, during, after acquistion), and when the analog filter is anti-aliasing (also choose from before, during, after acquistion)
2. Discuss whether you can consider a digital filter anti-aliasing
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
#9A anti-aliasing
### TO DO = "1. when does aliasing occur"

### TO DO = 2. how to use digital filtering against aliasing"

### TO DO="3a. abstract"

### TO DO="3b. troubleshooting"

### TO DO="3c. code"

### TO DO="4. what changes would you suggest?"

```

```python
#recording
from IPython.lib.display import YouTubeVideo
YouTubeVideo('l-T0m7k9lXc', width = 600, height = 450)


```

```python

```
