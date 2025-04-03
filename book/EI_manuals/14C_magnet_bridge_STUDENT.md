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
- experiment 14A: simulate a circuit to understand how differential mode is amplified and common mode is rejected
- experiment 14B: build a comparator (with hysteresis) circuit and explain its behaviour
- experiment 14C: build a magnetic sensor using instrumentation amplifer and understand how it works

Goal: Understand the use of differential mode, bridge sensors, a comparator (with positive feedback), and sensing a magnetic field. 

Structure of an experiment:
- Predict + Simulate (50): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Build + Measure (65): with your partner(group of 2)
- Evaluate (10): with a group of 4(per table)


# 14C: Magnetic bridge 



> <font color='blue'>Learning goal:</font>  Understanding and Using Comparators

> - **Function of Comparators**: Comparators are circuits that determine if a voltage is below or above a specified threshold. They typically use an OPAMP, but not in a linear manner. The output of a comparator has only two states: maximum positive or maximum negative, depending on the input voltage.
> - **Dealing with Noise Sensitivity**: In its basic form, a comparator circuit is very sensitive to signal-line noise. This can be improved by adding hysteresis.
> - **Demonstration**: The experiment will demonstrate how to implement hysteresis and observe the resultant behavior of the circuit.

<!-- #region -->
## BACKGROUND
> <font color='grey'>⏳ Estimated time: 10 min</font>

### 1. Exploring Sensor-Bridge Applications
In this experiment, we delve into the practical application of a sensor-bridge. Sensor bridges are particularly useful with sensors that exhibit higher sensitivity to secondary parameters other than the one being primarily measured. Here, we focus on a bridge constructed with resistors sensitive to magnetic fields, which are, notably, even more sensitive to temperature changes!

### 2. Balancing Sensitivities in the Bridge
To offset the varying sensitivities, the bridge is designed with two resistors having a negative coefficient for the targeted parameter (magnetic field) and two with a positive coefficient. This configuration enables the creation of a functional system, even though all resistors share the same sensitivity coefficient for temperature.

### 3. Instrumentation Amplifier vs OPAMP
While the schematic of an instrumentation amplifier may resemble that of an Operational Amplifier (OPAMP), their functionalities differ. The instrumentation amplifier, a specialized form of a differential amplifier, amplifies the voltage difference between its inverting and non-inverting inputs with an adjustable gain-factor. This gain is set using an external resistor (Rg).

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/LTS9_1a.png" width=800>
    <br>
    <em>Diagram: Sensor-Bridge Resistor Configuration.</em>
</div>

### 4. Understanding the Behavior under Magnetic Fields and Temperature
The diagram above illustrates how resistors react to magnetic fields, with all resistors demonstrating a positive coefficient (+dR) in response to temperature changes. In the absence of a magnetic field, all resistors maintain equal value, resulting in output voltages Ul and Ur being half the bridge voltage. Temperature-induced resistance changes, equal in all resistors, will similarly maintain this half-bridge voltage balance.

However, when exposed to a magnetic field, two resistors increase their resistance while the other two decrease it. This creates a small but significant deviation from the half-bridge voltage, yielding a differential voltage (Ur - Ul). This differential voltage is then amplified by the instrumentation amplifier to a usable level.

### 5. Bridge Output Voltage Dependency
For a full bridge configuration with four sensitive resistors, the differential output voltage can be expressed as:

$$ Uo = U_{bridge} \times \frac{dR}{R} $$


This equation indicates that the bridge's output voltage is directly proportional to its supply voltage. Increasing the bridge voltage enhances the signal strength! However, the maximum bridge voltage is constrained by the heat (power) tolerance of the bridge resistors.


<!-- #endregion -->

## ANTICIPATE : differential output voltage through the magnet bridge
> <font color='grey'>⏳ Estimated time: 20 min</font>

### Exploring the HMC1052 Resistor Bridge
In this section, we examine the HMC1052, which you will later use in the Implement & Investigate (I&I) setup. This device consists of a resistor bridge made up of four identical resistors, each with the same temperature coefficient. 

>**Crucially, this uniformity means that all resistors respond to temperature changes in a similar manner, thereby maintaining the stability of the bridge’s differential output voltage (Uout = U+out – U-out).**

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC14_5.jpg" width="600">
    <br>
    <em>Figure: Magnetic Bridge, Equivalent Circuit. dR represents changes due to temperature variation.</em>
</div>

### Mathematical Validation of Differential Output Voltage
Your task is to mathematically confirm the above statement by calculating the bridge’s differential output voltage under uniform resistance change. Consider the following scenario:
- All resistors (R1, R2, R3, and R4) in the bridge alter their resistance by an equal amount, denoted as dR.
- Assume the basic resistance value of each resistor (R1 = R2 = R3 = R4) is R.

**Objective**: Determine the differential voltage, which is the voltage difference between the “+out” and “-out” terminals of the bridge.

> **<font color='blue'>Hint:</font>**
To determine if the differential output voltage is indeed zero, consider the following approach:
>- Voltage Calculation for '-out': Measure the voltage across (R1 + dR). Express this voltage in terms of R and dR.
>- Voltage Calculation for '+out': Similarly, measure the voltage across (R3 + dR), and express it in terms of R and dR.
>- Calculating the Differential Output Voltage: Calculate the difference between the voltages at '+out' and '-out'. This will give you the differential output voltage.



```python
### TO DO ="derive that Uout does not depende on dR for the above circuit"
 
```

> **<font color = 'blue'> Hint: </font>** <br>
'-out' measures the voltage over (R1+dR); '+out' measures the voltage over (R3+dR). When you express these voltages in R and dR and calculate the difference, do you indeed find that the differential output voltage is zero?


### Calculating the Differential Output Voltage under Magnetic Influence
Now, let's shift our focus to the impact of a magnetic field on the resistor bridge. Unlike the previous scenario where dR represented changes due to temperature, here dR signifies the change in resistance due to a variation in the magnetic field.

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC14_6.jpg" width=600>
    <br>
    <em>Figure: Magnetic Bridge, Equivalent Circuit. Here, dR represents changes due to variations in the magnetic field.</em>
</div>

> **Key Assumption**: For any practical magnetic field, it can be safely assumed that dR << R. This means the change in resistance (dR) due to the magnetic field is much smaller than the basic resistance value (R) of each resistor.

**Task:** Apply the same method of calculation as you did for the temperature variation. Determine the new differential voltage, which is the voltage difference between the “+out” and “-out” terminals, considering the influence of a magnetic field on the bridge.


```python
### TO DO=" Derive how Uout depends on dR in the above (second) picture "
 
```

**Conclude:** Now, you should understand why this configuration is used to measure changes in magnetic field.

```python
### TO DO=" Explain how such bridge configuration can be used to measure a magnetic field "
 
```

## SIMULATE: Analyzing a simple instrumentation amplifier
> <font color='grey'>⏳ Estimated time: 20 min</font>

### Setting Up the Simulation
- **Download the Scheme**: Download the schematic named "week14C" from Brightspace. This schematic represents a simple instrumentation amplifier configuration, which, unlike the AD620 used in Implement & Investigate (I&I), lacks capacitors and inductors.
- **Run the Simulation**: Observe the behavior and output of the amplifier. What is the gain?

<div style="text-align: center">
    <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/LTS/14C_simple_diff_amplifier.PNG" width="800">
    <br>
    <em>Figure: Schematic of the circuit</em>
</div>


```python
### TO DO=" what is the gain of the week 14C simulation "
 
```

### Determining the Amplifier's Gain
- **Gain Formula**: The gain of this specific instrumentation amplifier can be calculated using the following formula:

  $$ G = \frac{R_d}{R_c} \left(1 + \frac{2 \times R_b}{R_a}\right) $$

  


-  **Identify resistors:** Your task is to match the resistors from the circuit (R1, R2, R3, R4, R5, R6, R7) to their corresponding parts in the gain formula (R_a, R_b, R_c, R_d).

| Scheme Sign | Formula Sign |
  | ----------- | ------------ |
  | R1 & R3     | ?            |
  | R2          | ?            |
  | R4 & R6     | ?            |
  | R5 & R7     | ?            |

- **Adjust and Observe**: Experiment by changing the value of one group of resistors in the simulation. Observe how this adjustment affects the output and, consequently, the gain of the amplifier.

```python
### TO DO="match R1234567 to Rabcd"
 
```

```python

#precap: 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('40dbfSzPY-Q', width = 600, height = 450)
```

<!-- #region -->
## IMPLEMENT & INVESTIGATE 1: Verifying the AD620 Amplifier Gain
> <font color='grey'>⏳ Estimated time: 25 min</font>

#### Equipment and Setup
To verify the gain of the AD620 instrumentation amplifier, you will need the following equipment:
- Testboard 15
- Function generator
- Agilent multimeter
- Power supply

#### Building the Instrumentation Amplifier Circuit
- **Circuit Implementation**: Assemble the instrumentation amplifier circuit on Testboard 15 as shown in the image below. Pay close attention to the component placement and connections.

  <div style="text-align: center">
      <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC14_2.jpg" width=800>
      <br>
      <em>Figure: Instrumentation Amplifier Implementation on Testboard.</em>
  </div>

- **Signal Generator Connection**: Connect the signal generator to the BNC connector on the testboard(first port). Simultaneously, connect it to the oscilloscope (on CHANNEL 2). Configure the function generator to output a sine wave with the following settings:
  - Frequency: 400Hz
  - Level: 20mVpp
  - No offset
  - Output Mode: High Z

  <br>

- **Oscilloscope Connection**: Attach the oscilloscope to U18 (probe-tip) and U19 (crocodile clip). Remember to use the probe for accurate measurements.

  <div style="text-align: center">
      <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14C_magnet_bridge.jpg" width="70%">
      <br>
      <em>Figure: Connection for Gain Verification.</em>
  </div>

- **Powering the AD620**: Ensure that the AD620 is properly powered before proceeding.
- **Having switches in correct position**: Check if power switches are both in +/- 12V position(down) and amplifier switch is turned ON (up).

#### Gain Verification
- **Calculating and Measuring Gain**: Using the AD620 datasheet (available at [AD620 Datasheet](https://nl.mouser.com/pdfdocs/AD620.pdf)), particularly the “Theory of operation” section, calculate the gain of the instrumentation amplifier for various settings of Rg (infinite, 5.5kΩ, 500 Ω, 50 Ω).
- **Implement and Record**: Measure the input (Uin) and output (Uout) voltages for each Rg setting. Record these values along with the calculated and measured gains in dB in the table below.
- **Finding the switch position**: Look at the resistors on the board (next to the switches) and read their bands.


#### Calculated and measured gain of the instrumentation amplifier AD620.

|Value of Rg[Ω]|Switch position (SW102)|Gain (See datasheet)|Uin|Uout|Gain measured|Gain measured [dB]|Remarks|
| :-           |               :-:     | :-:                | :-| :- | :-          | :-               | :-    |
| ∞            |                       |                    |   |    |             |                  |       |
|5.49kΩ        |                       |                    |   |    |             |                  |       |
|499Ω          |                       |                    |   |    |             |                  |       |
|49.9Ω         |                       |                    |   |    |             |                  |       |
<!-- #endregion -->

```python
### TO DO ="fill in the table, and derive the gain value"
 
```

> **<font color = 'blue'> Hint: </font>** <br>
Not seeing the amplification that you expected? Maybe you forgot to compensate for the probe.


### Observing the Effects of Reversed Connections at 20dB Gain

Proceed with the following experiment to explore the impact of connection reversal:

1. Adjust the amplifier's gain to 20dB. 
2. Carefully reverse the connections at U15 and U16. 
3. Once you have reversed the connections, examine both the input and output signals on the oscilloscope.
   > Pay close attention to any changes in the waveforms, particularly noting alterations in phase, amplitude, or other signal characteristics.

- **Observations**: What differences do you notice in the output signal compared to the input signal after reversing the connections?

```python
### TO DO ="What is the change due to reverse connections"
 
```

## IMPLEMENT & INVESTIGATE 2: Calibrate the bridge and the sensor
> <font color='grey'>⏳ Estimated time: 20 min</font>

### 1. Preparing the Board for Magnetic Field Observation
Before delving into the experiment with the magnet bridge, some preparatory steps are essential:

<br>
**Remove your previous setup from the board**. You can keep the connected coax cable.<br>
<br>

#### Sensor Reset
- **Understanding the SET/RESET Strap**: The sensor incorporates a built-in SET/RESET strap, essentially a coil surrounding the magneto-sensitive resistors. Regularly resetting these resistors is crucial as their values can drift over time.
- **Applying Current Pulses**: 
  - Use **SW101** to apply short current pulses, switching either to SET or RESET.
  - Remember to **return the switch to its middle position** after each use. Leaving the switch in SET or RESET will continuously generate reset pulses every second.

  <div style="text-align: center">
      <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14C_reset__Vbridge.jpg" width="70%">
      <br>
      <em>Figure: Sensor Reset Mechanism.</em>
  </div>

#### Adjust the bridge voltage
*We will now perform some measurements. But before doing them use R17 to adjust the bridge voltage to 5V. This can be observed at U4. Note: U2 is ground.*

- **Setting Bridge Voltage to +5V**: 
  - Initially, disconnect the BNC cable (connected to the function generator and oscilloscope) from the function generator. 
  - Connect it to the second port (BNC1) on the testboard.
  - Establish connections between U54 and U2 (ground), and between U53 and U4.
  - Use R17 to adjust the bridge voltage. Observe and fine-tune the voltage value to precisely 5V using the oscilloscope.

  > **<font color='blue'>Hint:</font>** Ensure that you are accurately measuring the correct voltage on the oscilloscope.


### 2. Observing and Measuring Current During Sensor Reset
#### Setting Up for Current Observation
1. First, disconnect the BNC cable from the channel BNC1 and reconnect it to the oscilloscope.
2. Set the function generator to output a sine wave with the following characteristics:
  - Amplitude: 5V peak-to-peak (Vpp)
  - Offset: No offset
  - Frequency: 1kHz
3. Apply the RESET trap by switching SW101 accordingly.
4. Connect U51 to U1A and U52 to U1B.

#### Qualitative Observation 
- Monitor CHANNEL 2 on the oscilloscope.
- **Question**: What do you observe on the oscilloscope when the RESET trap is applied?

Be aware that the function generator is effectively short-circuited by the copper tracks on the PCB between U1A and U1B. Consequently, the current through the circuit is primarily determined by the generator’s 50Ω output impedance.

#### Measuring the Current
- Take a handheld amperometer to measure the current flowing through the circuit during the RESET operation. Record the measured current value.
- **Question**: What did you observe when measuring the current?

#### Calculating the Injected Current
- Calculate the peak-to-peak amplitude of the injected current (𝑑𝐼𝑔𝑒𝑛) in the copper tracks. Assume there is no resistance in the wires for this calculation.
- **Comparison**: Compare your calculated value of 𝑑𝐼𝑔𝑒𝑛 with the value measured using the handheld amperometer.

```python
### TO DO=" your observation on amperemter and that converted to Ipp"
 
```

> **<font color = 'blue'> Hint: </font>**
In this circuit, there is a short circuit and therefore only one resistor if we assume wires are perfect. Use the input voltage and this one resistance to calculate the current.


## IMPLEMENT & INVESTIGATE 3: observe the magnetic field effect
> <font color='grey'>⏳ Estimated time: 25 min</font>

<br>
Now you have all the components needed to observe the magnetic field effect. Replace the amperemeter back with a wire. 
<br>

#### Setting Up the Experiment
- **Amplifier Input Connection**: Connect the input of the AD620 amplifier to the bridge. Use the following connections:
  - U15 to U11
  - U16 to U13
- **Adjusting Amplifier Gain**: Set the AD620 to a gain of 100x.
- **Observing Output on Oscilloscope**: Monitor the amplifier's output on the oscilloscope. *Tip: Rememeber how you connected to the scope in I&I1 ?*

  <div style="text-align: center">
      <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/14C_magnet_bridge.jpg" width=800>
      <br>
      <em>Figure: Magnetic Bridge Connection for Observation.</em>
  </div>
  
#### Calculating Transfer Impedance
- **Objective**: Calculate the overall transfer impedance (𝑑𝑉𝑈17/𝑑𝐼𝑔𝑒𝑛) in mV/mA of the sensor system based on the observed results.
- **Use Ideal Current**: Refer to the ideal (non-experimental) current value from the previous exercise for this calculation.

```python
### TO DO=" your derivation of the overall transfer impedance"
 
```


### Experimental Verification
1. **Sensor Sensitivity to Current Direction**:
   - Investigate whether the sensor is sensitive to the direction of current flow.
   - _Tip_: Think about how you can change the current direction in the circuit through the sensor.
   - Focus on the phase of the output, not only the amplitude.

2. **Sensitivity to DC Offset**:
   - Determine if the sensor is affected by DC offset in the input signal.

3. **Linearity of Magnetic Sensor**:
   - Assess the linearity of the magnetic sensor by examining the shape of the amplifier output signal.
   - _Tip_: Use a triangle wave as the input and observe how the output signal changes. How does this affect your understanding of the sensor's linearity?

```python
### TO DO="1.senstive to currect? 2.sensitive to DC offset? 3. linearity?"
 
```

#### 4. Calculating Resistance Variation
- **Peak-to-Peak Resistance Variation (dRpp)**: Calculate the peak-to-peak resistance variation dRpp(dR/R) of the bridge elements from the measured amplitude variation (dVU17) of the amplifier output.
- **Relative Variation (dRpp/R)**: Determine how large the relative variation is. Use the formula $$U_{out} = \frac{dR}{R} \times U_{bridge}$$ 

    Note that $dVU17 \neq U_{out}$.

#### 5. Comparing with Temperature Coefficient
- **Comparison with Temperature Coefficient**: Compare your calculated relative variation (dRpp/R) with the typical temperature coefficient of the resistors in the sensor, which is around 2500ppm/°C or equivalent to 2.5Ω/°C.

```python
### TO DO="Derive the dR/R value and compare it to the tempearature coefficient"

```

## **<font color = 'red'> OPTIONAL EXERCISE : <br> Analyzing the Impact of Bridge Voltage on Output Voltage</font>** 

<!-- #region -->
#### System overview
The schematic below illustrates the entire measurement system. From this representation, it's evident that the bridge voltage also significantly impacts the system's output.
  
  <div style="text-align: center">
      <img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/ELC/ELC14_8.jpg" width="800">
      <br>
      <em>Figure: Schematic of the Measurement System.</em>
  </div>

> #### Experiment Setup
>- **Function Generator Settings**:
>  - Adjust the function generator to emit a 5V peak-to-peak (Vpp) sine wave.
>  - Set the DC offset to 0V.
>  - Frequency should be set to 400Hz.

#### Adjusting and Measuring Bridge Voltage
**Bridge Voltage Adjustment**: Vary the bridge voltage (measured at U4, with U2 as Ground) to different levels - 1.2V, 2V, 3V, 4V, and 5V.<br>
Carefully observe the changes in the output voltage of the amplifier at each bridge voltage level.


#### Recording Observations
Fill in the table below with your observations

<!-- #endregion -->

<!-- #region -->


| Bridge voltage [V] | 1.2 (!)|2.0|3.0|4.0|5.0|
| :-                 |:-      |:- |:- |:- |:- |
| Output voltage [V] |        |   |   |   |   |


<!-- #endregion -->

```python
### TO DO=" room for additional comments on the table above"

```

<!-- #region -->
## COMPARE & CONCLUDE
> <font color='grey'>⏳ Estimated time: 10 min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


**to be checked off by a TA:**
1. Evaluate the instrumentation amplifier (I&I1): explain whether it behaves as expected (gain, removing common signal)
2. Compare the change in temp vs change in magnetic field, what can you conclude?
3. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
4. How do think this notebook could be improved
<!-- #endregion -->

```python
#14C magnet bridge
### TO DO="1 evaluate the instrumentation amplifier"

### TO DO="2 Compare the change in temp vs change in magnetic field "

### TO DO="3a. abstract"
 
### TO DO="3b. troubleshooting"
 
### TO DO="3c. code"
 
### TO DO="4. what changes would you suggest?"


```

```python
#recording 
from IPython.lib.display import YouTubeVideo
YouTubeVideo('oVPmAAvVhO4', width = 600, height = 450)
```
