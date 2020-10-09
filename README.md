# Midi-Vidi
A Python based midi image trigger program which listens for midi input and displays an image based on the midi note.

Images should be specified in the switcher array.

There are 3 effect filters, brightness, contrast, black and white:
```
'''
The brightness effect boosts the luminance of the pixel by the value brightness_val.
There is a limit of 255 - if the bightness value + the current color value > 255: set
value to 255.
'''
    
def fx_bright(color_val, bright_val):
    
    clip = 255
    z = color_val + bright_val
    
    if z > 255: 
        return clip
   
    else: 
        return z

    
'''
The Contrast function boosts the color by the contrast_val amount if it is above the pivot value (127)    
and cuts the value if below the pivot value. 
This function increases the difference between light and dark colors.
'''
def fx_contrast(color_val, contrast_val):
   
    out = 0
    
    if color_val > 127:
        
        out = color_val + contrast_val
        
        if out > 254:
            out = 254
        
        else:
            out = out
        
    elif color_val <= 127:
        
        out = color_val - contrast_val
        
        if out < 0:
            out = 0
        
        else:
            out = out
        
    return out

'''
The Noir function takes in all three rgb values and returns brightness value by averaging the three color channels. 
This has the effecet of converting the image to black and white .

We can also use the Luminance formula to return a more defined black and white image. To select the luminance method,
pass a True as the fourth variable
'''

def fx_noir(r, g, b):
    avg = (r + b + g) / 3
    return avg

def fx_noir(r, g, b, lum_true):
    if lum_true: 
        lum = (.21 * r) + (.72 * b) + (g * .07)     
        return lum
    else:
        return fx_noir(r, g, b)
```

Using the PIL library, midi-vidi pulls pixel data, processes it with the effects functions, and displays it on screen. 
This method allows for the conversion of highresolution images to lower resolutions.

# Midi Channel

If the midi triggers are not being recorded, there is a chance you are litening on the wrong channel.
You can use atool such as MIDI-OX to find the correct channel and set the mini_channel variable accordingly. 

# Trigger from DAW

To trigger from a midi-vidi from a DAW, use a virtual midi bus such as LoopBe1, configure a midi out channel to utilize your virtual midi bus and set the mini_channel variable accordingly.
