
## Introduction

The RAT 2 is an effect pedal that was first released in the 1970s and 80s. 
Despite its age, it remains incredibly popular today. Its enduring appeal, especially for beginners in pedal building, stems from its simple circuit design, which makes it an ideal for my first project.
For this project, all design work was carried out using KiCad, an open-source EDA software.

## Circuit analysis
It can be broadly categorized into four sections: Power Supply, Gain, Tone, and LED Control.

### Power supply section

This is the 9V voltage input section, and all other parts of the circuit receive their power from here. The diode conducts only when the voltage is reversed. When it conducts, all current flows through a 100-ohm resistor. This happens because, for reverse current, the resistance on that path is significantly lower. The 100uF capacitor is for 9V voltage stabilization, while the two 100k resistors form a voltage divider that converts the 9V supply to 4.5V. The 1uF capacitor then stabilizes the voltage from this voltage divider.

### Clipping (gain) section 
This is core section. The circuit uses an LM308 to form a non-inverting amplifier, for amplifying raw guitar signal. At the op-amp's output, there's a back-to-back diode clipping circuit. Signals exceeding the diodes' forward voltage are shunted to ground, which clips the upper part of the waveform. This is the principle behind the characteristic "chugging" sound of electric guitars. So, changing diodes is a common modification for distortion and overdrive pedals among guitarists.

### Tone, output section.
Following the back-to-back clipping stage, you'll find the tone knob (potentiometer). This component adjusts the resistance (R) in an RC filter to control the high frequencies of the output signal. After this, a JFET (5458) acts as a source follower, serving to adequately boost the signal's strength.
## Design and Implementation
### Schematic Creation
While the design largely follows the original, I encountered some resistance values that weren't available in my inventory. For these, I used series or parallel combinations to achieve approximate values. 
The original RAT 2 uses the LM308 op-amp, but I wasn't keen on using a chip so specifically limited to just this pedal. Therefore, I opted for the TL072, which is more commonly used and versatile in guitar effects. 
Since the LM308 is a single op-amp and the TL072 is a dual op-amp, I adjusted the pinouts accordingly. I also configured the circuit to deactivate the second op-amp of the TL072.
### PCB layout Creation

