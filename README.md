
## Introduction

The RAT 2 is an effect pedal that was first released in the 1970s and 80s. 
Despite its age, it remains incredibly popular today. Its enduring appeal, especially for beginners in pedal building, stems from its simple circuit design, which makes it an ideal for my first project.
For this project, all design work was carried out using KiCad, an open-source EDA software.

## Circuit analysis
It can be broadly categorized into four sections: Power Supply, Gain, Tone, and LED Control.

### Clipping section 
This is core section. The circuit uses an LM308 to form a non-inverting amplifier, incorporating several capacitor filters. At the op-amp's output, there's a back-to-back diode clipping circuit. Signals exceeding the diodes' forward voltage are shunted to ground, which clips the upper part of the waveform. This is the principle behind the characteristic "chugging" sound of electric guitars. So, changing diodes is a common modification for distortion and overdrive pedals among guitarists.
