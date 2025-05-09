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
-   experiment 2A: Investigate the AC/DC coupling and the AC frequency dependency with the oscilloscope
-   experiment 2B: Apply the correct triggering between function generator and oscilloscope
-   experiment 2C: Use the picopi to measure the Thevenin equivalent of a circuit

Goal: Understanding the use of an oscilloscope (voltage, triggering) and the use of the ALPACA 

Structure of experiment:
- Background+Anticipate (5+10 min): per person. This is homework.
- Simulate: not this time
- Implement & Investigate (15+10 min): with partner (group of 2)
- Compare & Conclude (10 min): with group of 4 (per table)

<!-- #region -->
# 2B: Triggering between function generator and oscilloscope
> <font color='blue'>Learning goal:</font> understand what triggering is, how you can use it with the oscilloscope and the function generator, and why it is useful.

## Background
> <font color='grey'>⏳ Estimated time: 5 min</font>

The trigger is a very useful function that allows us to control the way in which a signal is displayed by the oscilloscope. The trigger determines when the oscilloscope starts to acquire data and display a waveform. 

Check out the intro on the scope, focus on triggering: 
https://mwdocter.github.io/TeachBook/main/EI_manuals/intro_scope_Agilent.html


## Anticipate
> <font color='grey'>⏳ Estimated time: 10 min</font>

You can trigger either on the signal itself (a sine) or on the sync signal (a block wave). 
* Give an example when it is ok to use the signal for triggering
* Give an example when you need the sync signal for triggering
<!-- #endregion -->

```python
### TO DO="Examples when to trigger on the signal itself, and on the sync"

```

Watch the following movie to get a quick intro in what you will be doing in the measurements

```python
## precap ELC4

from IPython.lib.display import YouTubeVideo
YouTubeVideo('GB855wO8aG8', width = 600, height = 450)
```

<!-- #region -->
## Implement 1: Display the signals correctly, while triggering on signal 1
> <font color='grey'>⏳ Estimated time: 15 min</font>

### 1a Display the signals in an untriggered condition.  
* Generate a 1kHz sine wave of 2Vrms and connect the function generator's output to Channel 1 of the oscilloscope. 
* Connect the SYNC to Channel 2 of the oscilloscope. 

See https://mwdocter.github.io/TeachBook/main/EI_manuals/intro_scope_Agilent.html for the default settings. Use those settings.

Explore the different sources, which one works best for you?


### 1b Explore settings: MODE and SLOPE control
* With trigger MODE set to EDGE change the SLOPE control. 
* Observe the difference between the rising edge and the falling edge. 

When toggling between rising and falling edge, is the signal shifting in time, or inverting amplitude (+-)?

### 1c Explore settings: trigger level
* Set the trigger level to a high value (1.4 - 2.4V). 
* Now in the function generator, change the amplitude of the voltage from 2Vrms to 2Vpp and observe what happens to the waveform displayed. 
* In general, it is recommended not to set the trigger at a very high level.  Adjust the trigger level again, to get a steady image of the signal.




<!-- #endregion -->

```python
### TO DO="write down your notes on SLOPE control and trigger level, what do they do?"

```

## Implement & investigate 2: Display the signals correctly, while triggering on signal 2
> <font color='grey'>⏳ Estimated time: 10 min</font>

### 2a Trigger on the SYNC signal on Channel2
* Implement triggering on CH2 (SYNC) instead of CH1 (output signal)
* Lower the voltage of the output signal at the function generator by a factor of 10. 
* What happens to the signal on the scope screen?

Can you already figure the benefit of triggering on the SYNC instead of the signal?

### 2b Trigger on the SYNC signal on the external triggering channel
* Switch the cable with the SYNC signal from CH2 to external triggering. 
* You cannot observe the SYNC signal on the scope anymore, but you know its values. 
* Adapt the trigger SOURCE consequenctly. 

What is the benefit of using SYNC on the external trigger instead of on CH2? 


```python
### TO DO="write down your notes on triggering on the SYNC, and using the external trigger signal"

```

<!-- #region nbgrader={"grade": false, "grade_id": "cell-b192045c4dab7475", "locked": false, "schema_version": 3, "solution": false, "task": false} -->
## Compare & Conclude
> <font color='grey'>⏳ Estimated time: 10 min</font>

By now you know the drill:
* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:** <br>
Suppose the following setup: signal V_in is generated by the function generator, V_out is to be measured by the oscilloscope. V_out turns out to be a really small and noisy signal. Both V_in and V_out can be connected to the scope. 
* What would happen if you trigger on V_out?

1. Explain which 2 signals can be used for triggering on the scope, and how to connect them?
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved
<!-- #endregion -->

```python
#2B triggering
### TO DO="What would happen on V_out? 1. Explain which 2 signals can be used for triggering on the scope, and how to connect them?"

### TO DO="2a. abstract"

### TO DO="2b. troubleshooting"

### TO DO="2c. code"

### TO DO="3. what changes would you suggest?"

```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip

```python
## recording ELC4

from IPython.lib.display import YouTubeVideo
YouTubeVideo('lozkaZJ3aUI', width = 600, height = 450)
```

```python

```
