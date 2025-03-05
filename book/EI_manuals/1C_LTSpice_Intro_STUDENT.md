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
- 40 (20+20) min background+anticipate : per person. This is homework.
- 40 (30+10) min simulate + implement & investigate     : with partner (group of 2)
- 15 min compare&conclude                    : with group of 4 (per table)


# 1C: LTSpice simulations
(Design voltage dividers with LTSpice to change the output voltage)
> <font color='blue'>Learning goal:</font> get introduced to the LTspice program, and design voltage dividers


## Background (homework): install LTSpice on your laptop
> <font color='grey'>⏳ Estimated time: 20 min</font>

In this course we use LTSpice. LTspice is a circuit simulator made available by the company Linear Technology, which is part of Analog Devices now. It allows to quickly draw a schematic with general or specific components and simulate the circuits to perform DC, AC and transient analysis (=response to a step function). Simulation results are presented in a graphical way. 

LTspice can be downloaded from: https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html
It is available for Windows (http://ltspice.analog.com/software/LTspiceXVII.exe) 
and for Mac (http://ltspice.analog.com/software/LTspice.dmg). The windows version also runs on Linux under the Wine package.
Go to the website of LTSPice; download and install the program. 

## Anticipate: Does the arrangement of voltage dividers influence the output voltage
> <font color='grey'>⏳ Estimated time: 20 min</font>

In exercise 1A you measured a simple voltage, which can be used to define a Vout which is smaller than its Vin. In this exercise you will combine two voltage dividers, and determine whether arrangement of these two voltage dividers will affect Vout. 

* predict the  $\frac{Signal \, out}{Signal \, in}$ ratio for two individual voltage dividers, one with twice 10kΩ, the other one with 220Ω and 1kΩ 
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi2_2_individual_dividers.jpg" width="50%"/>
</div>

* Calculate the ratio $\frac{Signal \, out}{Signal \, in}$ for both the left and right voltage divider. Are they behaving the same?
<div>
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/a894a02ef157ca15d3d118eb243245eb912ba4f0/PicoPI/picopi2_1_combined_dividers.jpg" width="50%"/>
</div>
    
You might want to watch the following movie clips on how to approach a resistor circuit, and how to work wih a current splitter.

```python
### TO DO="your answer, Vout/Vin for the individual and combined voltage dividers"

```

```python
#theory	resistor circuit: approach 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('r2p5FDGEZSM', width = 600, height = 450)
```

```python
#theory	current splitter

from IPython.lib.display import YouTubeVideo
YouTubeVideo('UkF0ZLMI_0s', width = 600, height = 450)
```

In future assignments you will find the precap after the simulation, but since this is the intro to the LTSpice simulation, feel free to watch the video already here

```python
#precap on stacked voltage divider

from IPython.lib.display import YouTubeVideo
YouTubeVideo('vOUG4c1OQBU', width = 600, height = 450)
```

## Simulate: Use LTSpice to build the simulation
> <font color='grey'>⏳ Estimated time: 30 min</font>

In LTSpice you will often use the top tool bar. Follow the steps below to simulate the (stacked) voltage dividers you saw in Anticipate. 

<font color='magenta'> If you are working on MAC, your UI will look completely different. To access the menu, rightclick with your mouse. You should be able to access all the components below by navigating in the subsections of that menu. </font>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice.jpg" width=80%></img>


### Drawing, positioning and editing components
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice-121.jpg" width=40%></img> 

You need ___components___: 
* Press “R” or Left-click the resistor icon 
* move the resistor symbol to the drawing area
* left click again to create R1

In case you want to rotate the resistor, use the rotate 90 degrees and/or mirror option. You can also use ctrl+R.
Note that:
* the rotation is only in the clockwise direction
* for rotating an existing element, first click rotation, then click the element
* if you loose overview, hit space to zoom to full extent
  
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_rightclick_value_resistor.jpg" width=20%></img> 

___Edit___ the component's value by:
* Move the mouse over a resistor until the cursor changes into a “hand” symbol. 
* Then right-click to open the edit menu. 
* Set/Change the resistor value

Note: You can use the following prefixes: f/F (femto), p/P (pico), n/N (nano), u/U (micro), m/M (milli), k/K (kilo), meg/MEG (mega), g/G (giga), t/T (terra). So for a 1 kΩ resistor type 1k, and for 1Ω simply 1.

___Moving___ components
* left-click the move icon. 
* Position the “hand” cursor over the component and left-click. Now the component can be moved. 
* Click again to fix the new position. 
* Press ESC/right-click to stop moving components.

Note: any wire connections to the component will be broken. To have the connections move with the component: use the drag command (icon). 


### Placing the Voltage source
After placing the resistors, let's make it into a circuit. The advantage is that you can simulate, in order to see which voltages an currents exist in the circuit (and eventually tell something about your overall replacement resistance).

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice-122.jpg" width=40%></img> 
* Click the component  icon, type "volt" in the selection window and the selection will jump to voltage. Confirm by pressing OK.
* Drag the voltage-source symbol to the desired  position and left-click to place.
Note: to change the polarity or the orientation of the source, left-click the rotation icon one or more times before placement.
* Press ESC to stop placing voltage sources.
Setting voltage source values
* Hover the cursor over the voltage source until the “hand” cursor appears. Then click right to open the edit menu.  Set:  DC-value to 0 V and the series-resistance to 0.


### Placing the ground connection
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice-123.jpg" width=40%></img> 

Each circuit needs a potential reference (0V), all voltages will be calculated with respect to this point. This is done by placing a ground symbol.  
* Left-click the ground icon
* Move it to the right postion and left-click again.
* Press ESC to stop placing grounds.
* All ground symbols are connected to each other! 
* Don't forget to put the negative pin of the voltage source to ground as well. 



### Interconnecting the components
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice-124.jpg" width=40%></img> 

Once the components are (more-or-less) in the desired position and orientation you can draw the interconnecting wires.
* Left-click the wire icon.
* Left-Click on a connector-point (small square) of one component to start a wire.
* Left-Click on a connector-point of another component to complete the wire

Note: Only horizontal or vertical line segments can be drawn. When connection points are not on one line, the connection must be build up from horizontal and vertical segments. You can define corner points by clicking anywhere on the diagram.


### Labelling Nodes
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0_topmenu_LTSpice-125.jpg" width=40%></img> 

In order to help interpretation of results it is good practice to label the important nodes in the network. Otherwise LTspice does automatic naming, which is often unclear.
* Right-Click on each of the blue lines on top and choose  “Label Net” from the pop-up menu.
* Name them VA, VB respectively and connect the text-symbol that appears to the line.

**Note: do not use duplicate names, LTSpice will not be able to distinguish these labels (and identical names on components will return an error)**



> __<font color='blue'>Hint:</font>__
>   Your LTSpice scheme could already look similar to the following figure. For convenience you might want to use 1V as Vin (then the ratio Vout/Vin is easier to retrieve). 
    
> <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/stacked%20voltage%20divider.JPG" width=60%></img>     

<!-- #region nbgrader={"grade": false, "grade_id": "cell-918b28c70372bdbb", "locked": true, "schema_version": 3, "solution": false, "task": false} -->
### DC-Analysis
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0-simulate_Run.jpg" width=50%></img> 
* Follow the menu: Simulate > Edit Simulation Cmd. 
* In the pop-menu select the tab “DC op pnt” (DC at point) and click OK. 
* Left-click in the drawing area to drop the command “.op” 
 
* Click the RUN icon to start the simulation, the solution (currents and voltages) are shown in a pop-up window, similar to the above one
* From the currents and the nodes you can derive the total current, total voltage and therefore total resistance. 
* If you want to better recognise which node is where, you should name them. 
<!-- #endregion -->

### Saving your schematic
If you want to use your schematic later, you need to save it. Click on *File>Save as* and rename your file to "week1C" when saving. You will need your scheme later in this experiment. 


### Saving graphics
You can save your LTspice schematics and graphic results as graphics by selecting the corresponding windows and follow the menu: “Tools” > “Write image to .emf file”. Or use Windows snipping tools, or just print screen. 
<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/72f7c30357b9caa07cefd5a8c79735c2e179be67/LTS/LTS0-color_palette_background.jpg" width=25%></img> 
Tip: Normally the graphs have a black background which is OK on screen but inconvenient on paper. You can change the background to white by setting the Color preferences:
* Choose from the menu: Tools > Color Preferences 
* Select the Schematic Tab and set Selected Item = Background and set all three colors to 255, see figure.
* Select the WaveForm Tab and do the same.


## Implement&Investigate (simulate) 1: Do the stacked voltage dividers behave the same?
> <font color='grey'>⏳ Estimated time: 10 min</font>

Run the simulations you created in simulate and write down in the cell below which values you find for Vout?
Also upload a screendump, in which both your circuit plus the simulated outcome are shown.

```python
### TO DO=" your simulated Vout, for the 4 circuits"

```

```python
from ipywidgets import FileUpload
from IPython.display import Image

upload=FileUpload()
upload

```

```python
import os
#print(upload.value)
file_name="1C_1_all_R=1.jpg"
if upload.value!={}:
    with open(file_name,"wb") as f:
        try: f.write(upload.data[-1]) # python 3.7 Kernel code, not working on Vocareum
        except: f.write(upload.value[-1]["content"])  # python 3.8 Kernel code, not working on Vocareum

Image(filename=file_name, width="50%") 
#Image(os.path.abspath(file_name), width="50%")
```

## Compare&conclude
> <font color='grey'>⏳ Estimated time: 15 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 

**to be checked off by a TA:**
1. How/ why does the order of stacking matters?
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved

```python
#1C LTSpice intro
### TO DO="1. why does stacking matter?"

### TO DO="2a. abstract"


### TO DO="2b. troubleshooting"


### TO DO="2c. code"


### TO DO="3. what changes would you suggest?"

```

If you got stuck during the measurement, at the end of the lab assignment we offer you a movie clip with our recorded efforts in the lab. If you were successfull with measuring, then skip this movie clip

```python
## recording LTSpice 1

from IPython.lib.display import YouTubeVideo
YouTubeVideo('-HNAZ0IUNTs', width = 600, height = 450)
```

```python

```
