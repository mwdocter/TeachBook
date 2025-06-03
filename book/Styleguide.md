# styleguide (overall)

The overall structure of each experiment is BASIC:
- Background
- Anticipate
- Simulate
- Implement & Investigate
- Compare and Conclude. 

## Background
The background is a quick repeat of theory necessary to know before doing the experiment. This is to refreash your mind, and is not study material on its own. We refer you to the textbook for a theoretical, more structured way of gathering knowledge on electronic instrumentation. 

**This part is done prepatory and alone**

## Anticipate
It is better to think before act. During the prediction phase of the experiment you activate knowledge to predict the outcome of a measurement. This already gives you directions of what to expect (and what not to expect). 

**This part is done prepatory and alone**

## Simulate
Before doing measurements, you verify your predictions by running a simulation. If later on your measurement resuts are not in line with the prediction, you cannot deduct the cause of the inconsistency. With simulations, you can. 

**This part is done prepatory and alone**

## Implement & investigate
You will do measurements, often starting with running the experiment you predicted and simulated. In further steps you continu to vary parameters and investigate into more depth. 

**This part is done in groups of two in the studio classroom**

## Compare & Conclude
After doing the experiments you compare your predictions, simulations and measurement results. If not in agreement, you seek for explanations. If in agreement, you think about further improvements. 

**This part is done in groups of four, so two groups per table, in the studio classroom**

You answer the evaluating questions (per experiment manual) and then **discuss with a TA**. 
Students are encouraged to answer the questions and then write what they learned in the following format:
- 1. Write a brief abstract on what you learned (conclusion, useful graph), 
- 2. Which troubleshooting skills do you want to remember for next sessions, 
- 3. Which code do you copy for use in next sessions,
- 4. How do think this notebook could be improved


```python

```

# Styleguide (per week)
In the overall structure, the experiments (often ABC) are organised in sections of one week. 

Per section we have an:
* overall learning goal, 
* topic of the experiments: briefly describing the content of experiment A,B, and C
* (possibly): brief descriprion new equipment and circuits



# styleguide (per experiment)

## Layout:
- accessibility code
- title with # header
- learning goal, indented and in blue
- timing divided in BAS (per-class) and I and C (in class)
- Subsections named Background, Anticipate, Simulate, Implement & Investigate, Compare & Conclude. 

- youtube movies
- images
- fonts

## accessibility code
For accessibility each will start with a piece of code for the accessibility of the notebook (.ipynb file). You can adjust the width of the text with the function provided below.
- You can toggle the auto-numbering of the sections in the outline toolbox (sidebar or topbar).
- You can toggle the code line numbers in the dropdown menu of the "view" button in the topbar. 
- You can collapse/expand a cell by clicking the blue bar on the left side of the cell.

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

## title, goal, structure

The title starts with a single hashtag plus space. Later sub sections go with ##, ###, etc. 

The goal should be clear, consise, condensed, preferably with an active verb, specifics (such as device and circuit). 

the structure shows the timing of the experiment, and is split in time alone (BAS) time in class with group of 2 (I&I) and time to evaluate with a group of 4 (C&C). 

### Example 1A: Digital multimeter + voltage divider
> <font color='blue'>Learning goal:</font> understand how to use the DMM, how it behaves as part of a (voltage divider) circuit, and what its limitations are.

In each assignment the estimated time you will spend per part is given. If you spend much more time than that, it is a clear sign to ask for help sooner. 

**Structure + Timing of this experiment**: 
- 30 (10+10+10+0) min Background+Anticipate+Simulate  : per person. This is homework: finish it before the 4 hours practicum session
- 55 (10+30+15) min implement & investigate: with partner (group of 2)
- 10 min compare&conclude                  : with group of 4 (per table)



### subsections format
- subsections start with ##
- per subsection give an estimated time, indented in grey. Example: > <font color='grey'>⏳ Estimated time: 10 min</font>
- we want as little cookbook recipe following as possible. 
- add guiding questions



```python
### youtube movies: example code

from IPython.lib.display import YouTubeVideo
YouTubeVideo('RSAVxhfckII', width = 600, height = 450)
```

<!-- #region -->
### images

Inserting an online image, in our case on an open gitlab repository is done with html code. Have a look at the code of this block, and you see:
- '<img': start of image
- src: source name
- width: adjusting the width of the image
- '<\img>': closing the image


<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC1b%20voltage%20divider.jpg" width=30%></img>


<!-- #endregion -->

### fonts
you can make different color or size fonts:
- first define the font color 
- then write the text, or even copy paste an icon
- then close/remove the different font color with '</font>'

While Jupyter notebooks are quite forgving in not closing fonts, markdown files are not.

Example: 
<font color='grey'>⏳ Estimated time: 30 min</font>

- Making text in markdown italic is done by placing it between _underscores_
- Making text in markdown bold is e by placing it between **double astericks**




### Inserting student's images
In some cases an answer can be best illustrated with a picutre, either drawn or a photo taken from the setup, or oscilloscope screen, or screendump of the laptop. Then the following code is most udesfull, which need to be in two consecutive cells:

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

## Markdown versus html

There are two ways of having dropdown menu's. 

Inside a markdown file (not ipynb) you can best use admonition like below. 

In ipynb you need to use details from html, as the admonition is not visible. 
    
```{admonition} Numeric answer
:class: dropdown, seealso
$V_{Th}$=20 V 
$R_{Th}$=20 k$\Omega$ 
You could still calculate $I_{no}$ and verify whether this is equal to $\frac{V_{Th}}{R_{Th}}$
```
<details>
  <summary>Epcot Center</summary>
  <p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p>
</details>
```python

```
