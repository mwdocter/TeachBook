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
- Experiment 7A: Use NI-DAQ to acquire analog signals 
- Experiment 7B: Write a function that calculates the fourier transform 
- Experiment 7C: Receive and decode Morse code

Goal: Acquiring analog signals with NIDAQ, plus writing and testing code for the Fourier transfrom and to decode the Morse signal (from 6B)

Structure of an experiment:
- Anticipate + Simulate (10+20+20): per person. This is homework and should be finished **before** you start your 4 hours practicum session
- Implement + Investigate (30 min): with your partner(group of 2)
- Compare + Conclude (10 min): with a group of 4 (per table)



# 7C: Morse reception

> <font color='blue'>Learning goals:</font> You will learn how to receive a signal, and you will write an inverse code 




## BACKGROUND: Receiving a signal
> <font color='grey'>⏳ Estimated time: 10 min</font>

In exercise 6B you send out a morse code, now it is time to receive and decode it. Let's break that into steps

1 - **acquisition**: you will have to acquire the signal, turn it from visual light back into a recorded signal. 

2 - **analyzing the data**: the recorded signal might be noisy, while the input was binary on/off. Therefore, the first step to adapt is to threshold, and make it binary (on/off) again. The most simple way is a *signal>threshold*, however you can have smarter approaches (like https://pubmed.ncbi.nlm.nih.gov/34036291/, AutoStepfinder: A fast and automated step detection method for single-molecule analysis). 

3 - The next step is to **translate** the binaray times signal into dash, dots, long and short pauses. 

4 - And the last step is to **lookup** the dash-dot signal into alphabet again. 

While the measurement is someting to do in the lab, it is good practice to have your code ready on beforehand, and test it on dummy data. This is to be done in the anticipate section (write the reverse code), and in simulate (write code to go from an analog timed signal to a dash-dot-long/short pause string). 

```python
#change the port to your port: if it gives an error, it also suggest you the best COM number
%serialconnect to --port="COM3" 

# note: we run this file with alpaca kernel, but the analysis will be done in a python kernel (with %python)
# since the implementation of numpy in alpaca_kernel is limited
```

Some code from 6B is copied here for your convenience. Also a reversed dictionary is given. 

```python
%python
import numpy as np
import matplotlib.pyplot as plt
# from https://www.geeksforgeeks.org/morse-code-translator-python/
# Dictionary representing the morse code chart #dict will be explained in later courses, just use the code for now
MORSE_CODE_DICT = { 'A':'.-', 'B':'-...',
                    'C':'-.-.', 'D':'-..', 'E':'.',
                    'F':'..-.', 'G':'--.', 'H':'....',
                    'I':'..', 'J':'.---', 'K':'-.-',
                    'L':'.-..', 'M':'--', 'N':'-.',
                    'O':'---', 'P':'.--.', 'Q':'--.-',
                    'R':'.-.', 'S':'...', 'T':'-',
                    'U':'..-', 'V':'...-', 'W':'.--',
                    'X':'-..-', 'Y':'-.--', 'Z':'--..'} #,
#                     '1':'.----', '2':'..---', '3':'...--',
#                     '4':'....-', '5':'.....', '6':'-....',
#                     '7':'--...', '8':'---..', '9':'----.',
#                     '0':'-----', ', ':'--..--', '.':'.-.-.-',
#                     '?':'..--..', '/':'-..-.', '-':'-....-',
#                     '(':'-.--.', ')':'-.--.-'}

# adapted from https://stackoverflow.com/questions/28142019/python-morse-code-pausing
def encodeMessage(m):
    message = m.upper().strip()
    encodedMessage =''
    isInWord = False

    for ch in message:
        if isInWord:
            if ch in MORSE_CODE_DICT:
                encodedMessage += 'l'+ MORSE_CODE_DICT[ch]
            else:
                encodedMessage += 'w'
                isInWord = False
        else: # not in word
            if ch in MORSE_CODE_DICT:
                encodedMessage +=  MORSE_CODE_DICT[ch]
                isInWord = True
            else:
                pass    # nothing to do
    return encodedMessage

```

```python
%python
#from https://geekflare.com/python-morse-code-translator/
def reverse_mapping(mapping):
    reversed = {}
    for key, value in mapping.items():
        reversed[value] = key
    return reversed

REVERSED_MORSE_CODE_DICT=reverse_mapping(MORSE_CODE_DICT)


```

Now you can use two dictionaries to back and forth between morse and text. 
* Note: the line of code below shows you how to add strings together, and how to extract values from the dictionaries. 

```python
%python
print(MORSE_CODE_DICT['A'] + '    '+ REVERSED_MORSE_CODE_DICT['.-'])
```

## ANTICIPATE: 
> <font color='grey'>⏳ Estimated time: 20 min</font>

In anticipate you will look at the steps required to get from the recorded signal to 1. a binary signal, 2. a (received) morse code. 

The code just below is simulating a received signal, such that you can write and test your code to get to the morse code (with dots and dashes)


### PART 1: turning words -> morse -> binary -> voltage value and add noise

```python
%python # has to run in python kernel, since alpaca has no np.append or np.random 
%matplotlib inline # this is needed to show the plot in the notebook

## simulation code for making a binary signal
AA=encodeMessage('ha ha this should work'.upper()) # first make some input message

print(AA) # your message encoded in morse code
binar=np.zeros(1)
for ii in AA:
    #print(ii)
    if ii=="-":   binar=np.append(binar,[1,1,1,0])
    elif ii==".": binar=np.append(binar,[1,0])
    elif ii=="l": binar=np.append(binar,[0,0])
    elif ii=='w': binar=np.append(binar,[0,0,0,0,0,0])
print(binar) # your message encoded into binary code

# now transfer it to values between 0 and 3 V, plus add noise
binar2a=binar*3+0.6*(np.random.random(len(binar))-0.5)
plt.plot(binar2a)
```

### PART 2: turning noisy voltage back to binary


Now it is up to you to add to the code below, to extract a series of 0 &1 (binar3) from binar2a
> <font color='blue'>Hint:</font> use a threshold. Think what would be a logical threshold. It might be helpful to go back to previous code and print statements to see how to binar and binar2a look. Rememebr you want to threshold binar2a so it ends up looking like binar once again

```python
%python
def signal2binary(binar2a):
### TO DO=" make binar2a into binary (binar3), do some thresholding (fixed number, or adapted to np.max(binar2a)?)"

    return binar3

binar3=signal2binary(binar2a)
print(binar3*1)


```

### PART 3: turn binary -> morse


After making it into 0&1, write a function which translates the timesignal (binar3) into a message (with -.lw)


```python
%python
def timesignal2message(binar3):
    BB='' #empty string, which you can add letters to with BB+=''
    ii=0
    while ii<len(binar3): # while loop runs over the 0&1s
       # print(ii,binar3[ii:ii+2]) # no need to print, but be inspired by comparing to at least two consecutive binary values
       # you could search for [0,1,0], [0,1,1], [0,0,0] and [0,0,0,0,0] or something alike 
        # remember! if you want to find 0,0,0,0,0 make sure you don't exclude it first by "seesing" 0,0,0
### TO DO ="write some if-statements to check the binary number(s), and add a corresponding letter (lw.-) to BB"





        ii+=1
    
    return BB
#test if everything worked
BB=timesignal2message(binar3)
print(BB)
```

<!-- #region -->
## SIMULATE: 
> <font color='grey'>⏳ Estimated time: 20 min</font>

The last step is to decode the dots and dashes Morse code into English. Try out the function below. 

Does your decodedmessage read 'ha ha this should work'? 


If not, double check your `signal2binary` or `timesignal2message` function.
<!-- #endregion -->

```python
%python
def decodeMessage(AA):
    message=''# already initiate an empty string, which you'll later append, with str.append()
    # part 1a: first split into words and letters
    words = AA.split("w") # string.split(break) will make a list, with each entry of the list being a word
    for word in words: # loop over all words, per iteration you get one word
        letters = word.split("l") # split all letters within one word
        for letter in letters: # for one letter 
            message+=(REVERSED_MORSE_CODE_DICT[letter]) #add the found English letter
        message+=' ' # put a space between words
    return(message)

```

```python
%python
CC=decodeMessage(BB)
print(CC)
```

Do you think the `decodeMessage` will always work? 

Try it on the code below, where we altered one data point.

```python
%python
binar3[49]=1
BB=timesignal2message(binar3)
print(decodeMessage(BB))
```

There will be an error if the Morse code is not in the REVERSED_MORSE_CODE_DICT.

In order to still be able to read out the other part of the signal, we can use an extra bit of code: `try` & `except`. 

- The `try` executes the code, and if there's no error it continues after the `except` case. 
- If the `try` gives an error, then the code within the `except` block is executed. 

In the code below, we try to find the reversed Morse code, but if it can't find an existing letter, it puts in a question mark. 

Run the code below, and see whether your `binar3` can be read. 


```python
%python
# use try and except structure to catch errors in the binary signal and replace them with a question mark
def decodeMessage_adapt(AA):
    message=''# already initiate an empty string, which you'll later append, with str.append()
    # part 1a: first split into words and letters
    words = AA.split("w") # string.split(break) will make a list, with each entry of the list being a word
    for word in words: # loop over all words, per iteration you get one word
        letters = word.split("l") # split all letters within one word
        for letter in letters: # for one letter 
            try: message+=(REVERSED_MORSE_CODE_DICT[letter])
            except: message+='?'
        message+=' '
    return(message)
#print(decodeMessage(AA))
```

```python
k%python
binar3[49]=1
BB=timesignal2message(binar3)
print(decodeMessage_adapt(BB))
```

## IMPLEMENT & INVESTIGATE 1: find the optimal position for the photodiode

> <font color='grey'>⏳ Estimated time: 30 min</font>

### I&I1a. build the photodiode circuit

For receiving the signal, you do need to build a small circuit which consist of:

- a photodiode, 
- resistors 
- and an opamp type TL072 (you can see the name on top of the opamp.  *(The theory on such operational amplifier will be given in octal 3)*

At this point the circuit is given, for you to use it. 

- Use a small (black) breadboard to build the circuit. 

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/7C_photodiode.jpg" width=70%></img>

Since the opamp is probably new to you, and the pins are close and difficult to read, 
just below you can find a zoomed in version of the opamp circuit on the smaller breadboard to help you install it correctly:

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/main/PicoPI/7C photodiode zoom in.jpg" width=50%></img>

<img src="https://gitlab.tudelft.nl/mwdocter/nb2214-images/-/raw/7b873596c01a500f66c42c89508ee5aa384b6335/voltammetry/opamp_dual_layout+component.jpg" width=50%></img>

Please note that:
* the photodiode has a long and short pin. The long one goes to -12V, the short one to opamp-pin 2 (the inverting input)
* the 220k resistor goes between opamp-pin1&2 (inverting input and output)
* the 220 $\Omega$ resistor is in between opamp-pin1 and AMP0-signal+
* the non-inverting input goes to ground. 
* the -12V and +12 V input are connected to the supply pins below the breadboard. 




### I&I1b. run the code, and optimize photodiode alignment
The code below is to allow you to position your receiving photodiode circuit optimally with respect to the LED on the Alpaca+picopi. Try it as many times as you want, until you receive a high enough output (to be able to distinguish a high and low signal).

You might need to shield the photodiode + led from the ambient light as otherwise its light might not be strong enough to be detected

```python
%serialconnect to --port="COM3" 
%plot --mode live
# first code: rough switching on-off. 
# Can be used to check whether the positioning of the photodiode on top of the led is correct

import time
import numpy as np
import matplotlib.pyplot as plt
NN=0 # later on use to give images an incrementing name

from machine import ADC, Pin
from functiongenerator import FuncGen, DC, Sine

# Instantiate a measurement pin Ain0 for the output signal
adc0 = ADC(26) 

led = Pin(25, Pin.OUT)

NUM_SAMPLES = 400
DELAY_MS = 100

output_signal = np.zeros(NUM_SAMPLES)

onoff=0
on_off=np.zeros(NUM_SAMPLES)
led.value(onoff) # move photodiode over de led to see whether the signal is high enough
for ii in range(NUM_SAMPLES):
        # Take a sample ever 20 ms (e.g. at 50 Hz)
        sample = adc0.read_u16()
        output_signal[ii] = sample
        
        if (ii % 5) == 0:
            # Plot 1 in 5 samples to the live plotter (e.g. with a frequency of 10 Hz)
            plt.liveplot(sample*5.035E-4, labels = ['Output [V]'])
                
        time.sleep_ms(DELAY_MS)
        if (ii%50)==0:
            onoff=not(onoff)
            led.value(onoff)
        on_off[ii]=int(onoff)
print('Acquisiton done!')
led.value(0)
```

## IMPLEMENT & INVESTIGATE 2: find the optimal position for the photodiode

You are given the binary array 'mesbin', which you send out and receive at the same time with the code below. Run it, and discover which message was send to you. 

```python
# initializing mesbin
import numpy as np

mesbin= np.array([0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,1,1,0,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,0,0,0,1,0,1,0,1,0,0,0,0,0,0,0 ,1 ,0 ,1 ,0 ,1 ,0 ,0 ,0 ,1 ,0 ,1, 0, 1, 0, 1, 0, 0 ,0 ,1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0 ,0 ,0 ,0 ,0 ,0 ,1 ,0 ,1 ,1 ,1, 0, 1, 1, 1, 0, 0 ,0 ,1 ,1, 1, 0 ,1 ,1 ,1 ,0 ,1 ,1 ,1 ,0, 0 ,0 ,1, 0, 1 ,1 ,1 ,0 ,1 ,0 ,0 ,0 ,1, 1, 1, 0, 1, 0, 1, 1, 1, 0])
```

```python
%python #we need to have it in python as well, to later on compare to the measured signal
mesbin= np.array([0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,1,1,0,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,0,0,0,1,0,1,0,1,0,0,0,0,0,0,0 ,1 ,0 ,1 ,0 ,1 ,0 ,0 ,0 ,1 ,0 ,1, 0, 1, 0, 1, 0, 0 ,0 ,1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0 ,0 ,0 ,0 ,0 ,0 ,1 ,0 ,1 ,1 ,1, 0, 1, 1, 1, 0, 0 ,0 ,1 ,1, 1, 0 ,1 ,1 ,1 ,0 ,1 ,1 ,1 ,0, 0 ,0 ,1, 0, 1 ,1 ,1 ,0 ,1 ,0 ,0 ,0 ,1, 1, 1, 0, 1, 0, 1, 1, 1, 0])
```

### I&I2a. Run the code; SEND 'mesbin' via LED and RECEIVE signal via a photodiode

Run it, and discover which message was sent to you. 

```python
%serialconnect to --port="COM3" 
%plot --mode live
# second code: send+receive morse code 
# Can be used to check whether the signal is good enough to read in the code

import time
import numpy as np
import matplotlib.pyplot as plt
from machine import ADC
from functiongenerator import FuncGen, DC, Sine

NN=0 # later on use to give images an incrementing name


# Instantiate a measurement pin Ain0 for the output signal. ADC(26) means that the signal is read from pin 26.
adc0 = ADC(26) 

# Instantiate a pin for the LED, Pin take the number of the pin, Pin.OUT means it is an output pin.
led = Pin(25, Pin.OUT)

# Define the number of samples to take, the delay between samples 
NUM_SAMPLES = 400
DELAY_MS = 100

# Create an array to store the output signal
output_signal = np.zeros(len(mesbin))

# Create I/O and plot data
for ii in range(len(mesbin)):
    
        # Take a sample ever 20 ms (e.g. at 50 Hz)
        led.value(mesbin[ii])
        sample = adc0.read_u16()
        output_signal[ii] = sample
#         if (ii % 5) == 0:
#             # Plot 1 in 5 samples to the live plotter (e.g. with a frequency of 10 Hz)
#             plt.liveplot(sample*5.035E-4, labels = ['Output [V]'])                
        time.sleep_ms(DELAY_MS)
print('Acquisiton done!')

# Turn off the LED
led.value(0)

```

```python
plt.plot(output_signal)
```

### I&I2b. transfer the signal from alpaca_kernel to python3 kernel
Because the analysis is running in the %python cells, we need to transfer between the ALPACA kernel and the python3 kernel. In order to do so, run the code below. 

```python
#threshold first, then copy the signal from below and paste it back into 7C1 to further analyze
thresh=np.median(output_signal) #you can try np.mean instead is your signal is very noisy
BB=(output_signal>thresh)*1

plt.plot(BB)
string='signal=['
for ii in range(len(BB)):
    string+=str(BB[ii])
    string+=','
string+=']'
print(string)
```

### I&I2c. Check whether the signal has been correctly received
Copy&paste the output of the previous cell: `signal=...`, 
like it is done below for `signal_teacher` (which is real binarized data, for which the background light might have been too much)

```python
%python
### TO DO='paste here your signal found in the alpaca kernel"

signal_teacher=[0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,1,1,0,0,0,0,0,0,0,1,1,1,1,0,0,1,0,1,0,1,0,1,0,0,0,1,0,1,0,0,0,1,0,1,0,1,0,1,0,0,0,0,1,1,0,1,0,1,0,0,0,1,0,1,0,1,0,1,0,0,0,1,1,1,0,1,1,1,1,1,1,1,1,0,0,1,0,1,0,1,1,1,0,0,0,1,0,1,1,1,0,1,0,1,0,0,0,1,1,1,0,1,0,1,0,0,0,0,0,1,0,1,0,1,1,1,0,1,1,1,0,0,0,1,1,1,0,1,1,1,0,1,1,1,0,0,0,1,1,1,1,1,0,1,0,0,0,1,1,1,0,1,0,1,1,1,0,]

#signal_teacher is far from perfect, found at an early test of the code

```

convert both `mesbin` and the found `signal`, and read whether the acquisition and coding/decoding went well.

```python
%python
print(decodeMessage_adapt(timesignal2message(mesbin)))
print(decodeMessage_adapt(timesignal2message(signal)))


```

## Optional I&I3: use the code below, to receive a secret(?) message send out by your team member (using 6B code)

```python
%serialconnect to --port="COM3" 
%plot --mode live
# second code: send+receive morse code 
# Can be used to check whether the signal is good enough to read in the code

import time
import numpy as np
import matplotlib.pyplot as plt
NN=0 # later on use to give images an incrementing name

from machine import ADC
from functiongenerator import FuncGen, DC, Sine

# Instantiate a measurement pin Ain0 for the output signal
adc0 = ADC(26) 

NUM_SAMPLES = 400
DELAY_MS = 100

output_signal = np.zeros(len(mesbin))
for ii in range(len(mesbin)):
    
        # Take a sample ever 20 ms (e.g. at 50 Hz)
        sample = adc0.read_u16()
        output_signal[ii] = sample
      
        time.sleep_ms(DELAY_MS)
        
print('Acquisiton done!')

#threshold first, then run the rest of the code copy the signal and paste it back into 7C1
thresh=np.median(output_signal)
BB=(output_signal>thresh)*1

plt.plot(BB)
string='signal=['
for ii in range(len(BB)):
    string+=str(BB[ii])
    string+=','
string+=']'
print(string)

```

```python
%python
### TO DO="signal= copy paste your found value here" 

print(decodeMessage_adapt(timesignal2message(signal)))
```

<!-- #region -->
## EXPLAIN & EVALUATE
> <font color='grey'>⏳ Estimated time: 10  min</font>

* Wait till all (4) group members finish their observation
* Compare your results with your other group members. 
* If your results agree, and are in line with all predictions, then talk to a TA and get checked off
* Otherwise, so if your results do not agree, or your results are not in line with your predictions, then first discuss amongst your group before getting a TA. 


Reflect on how accurately you could measure the send out morse message in light. 
- how stably did you position the receiving photo diode opposite of the led?
- how did you shield the photodiode from light outside the LED
- if the received message is not the same as send out, try to optimize further with the above reflecting questions in mind. 

**to be checked by the TA**: 
1. Discuss the (possible) difference between send out and received signal, and its causes
2. exit card: 1. Write a brief abstract on what you learned (conclusion, useful graph), 2. Which troubleshooting skills do you want to remember for next sessions, 3. Which code do you copy for use in next sessions,
3. How do think this notebook could be improved
<!-- #endregion -->

```python
#7C receive Morse
### TO DO="1. discuss the difference between send out and received signal, include do/don'ts"

### TO DO="2a. abstract"

### TO DO="2b. troubleshooting"

### TO DO="2c. code"

### TO DO="3. what changes would you suggest?"

```

```python
# no recording, an example recorded signal is already given as signal_teacher. 
# compare this with the input signal, and you'll see what kind of signal is possible

```
