
## Why I made this 
I’d been playing electric guitar for years, and like many people, I used digital multi-effect processors because they’re more affordable and convenient than buying several separate analog pedals.

A few months ago, I found out that some Chinese analog pedals were surprisingly affordable, so I bought one out of curiosity — and got completely hooked by the physical feel. So I bought a few more, and eventually even tried assembling a DIY soldering kit.

After finishing the kit, I realized that if I could design the PCB myself, I could build these pedals even more cheaply and customize them the way I wanted. That’s what led me to learn PCB design from scratch.
### The RAT2 - Simple but Effective

The RAT2 is a distortion pedal first released in the late 1970s.  
Despite its age, it’s still incredibly popular today. Its enduring appeal — especially among beginners in pedal building — comes from its simple and elegant circuit design, making it a perfect choice for my first project.

All schematic and PCB design work was done using KiCad, an open-source EDA software.
## Circuit analysis
It can be broadly categorized into four sections: Power Supply, Gain, Tone, and LED Control.
	<p align="center">
  <img src= asset/rat2_sch.gif width="80%" height="80%">
</p>

### Power supply section

This is the 9V input section, from which the rest of the circuit receives power. The diode conducts only when the polarity is reversed. When that happens, all current flows through a 100-ohm resistor, which acts as a fuse. This happens because, for reverse current, the resistance on that path is significantly lower. The 100uF capacitor is for 9V stabilization, while the two 100k resistors form a voltage divider that converts the 9V supply to 4.5V. The 1uF capacitor then stabilizes the voltage from this voltage divider.

### Clipping (gain) section 
This is core section. The circuit uses an LM308 to form a non-inverting amplifier, for amplifying raw guitar signal. At the op-amp's output, there's a back-to-back diode clipping circuit. Signals exceeding the diodes' forward voltage are shunted to ground, which clips the upper part of the waveform. This is the principle behind the characteristic "chugging" sound of electric guitars. So, changing diodes is a common modification for distortion and overdrive pedals among guitarists.

### Tone, output section
Following the back-to-back clipping stage, you'll find the tone knob (potentiometer). This component adjusts the resistance (R) in an RC filter to control the high frequencies of the output signal. After this, a JFET (5458) acts as a source follower, serving to adequately boost the signal's strength.

### LED Control section
You might know this circuit as 'The Millennium Bypass.' Usually, you'd need a 3PDT switch for a true bypass with an LED indicator control. But back in the day, 3PDT switches were hard to find. That's why pedals like the RAT used this bypass method. Normally, a 2PDT switch only gives you true bypass. However, by adding a JFET, this design also lets you control the LED's on/off state.

Below, I've sketched out simple schematics for three different true bypass methods, all in their bypass mode. Most of these are pretty straightforward, but the Millennium Bypass has a counter-intuitive quirk with its LED section: the LED actually goes off when LED section is connected to the circuit. I'll explain exactly why that happens further down.
<p align="center">
  <img src=asset/bypass_types.png width="80%" height="80%">
</p>

### The Millennium Bypass

<p align="center">
  <img src=asset/Millennium.gif width="50%" height="50%">
</p>
As you can see in the picture above, when the circuit is in bypass mode, the gate of the 5458 JFET is pulled directly to GND (Lime line). Because the gate is at GND, Vgs is below 0V(Vs>0, Vg=0), so the JFET channel is largely closed. 
Even though 9V is still connected to the gate through the 10MΩ resistor, the high resistance prevents it from significantly affecting the gate voltage, as long as the gate is pulled to GND.

On the other hand, when the effect is in engage mode (the lime route is disconnected), the gate is only pulled up to 9V through the 10M resistor (Red line). This creates a sufficiently positive Vgs (Vgs ≥ 0), ensuring that the JFET channel opens.

## Design and Implementation

### Schematic Creation
	
 <p align="center">
  <img src=asset/kicad_sch.png width="80%" height="80%">
</p>
※ In advance, I’d like to acknowledge a few mistakes in this post.
Two issues occurred: First, I accidentally reversed the source and drain of the JFETs. Second, I swapped pins 1 and 3 of the gain potentiometer.
I was already aware of the latter, as the gain knob worked in reverse — not a major issue. However, the reversed source and drain on the JFETs are more significant, as this can prevent them from working properly.
Fortunately, despite this, the circuit functioned without any noticeable problems (even during debugging), so I didn’t realize the mistake until now.
Also, in this circuit, the JFETs don’t play a critical role — they’re only used as buffers and for LED switching.
So, please read the following text with the understanding that the JFET pin swap did not affect the circuit's operation.

While the design largely follows the original, I encountered some resistance values that weren’t available in my inventory. To address this, I used series or parallel combinations to approximate the required values.
The original RAT 2 uses the LM308 op-amp, but I wasn’t keen on using such a specific chip that’s mostly associated with just this pedal. So, I opted for the TL072 instead, which is more versatile and commonly used in guitar effects.
Since the LM308 is a single op-amp and the TL072 is a dual op-amp, I adjusted the pinout accordingly. I also configured the circuit to deactivate the second op-amp in the TL072.

### PCB layout Creation

<p align="center">
  <img src=asset/early_ver_pcb.png width="50%" height="50%">
</p>

The picture above shows an early version of the PCB layout. 
Since I had to learn PCB design on my own, at first I wasn’t sure how large to make the board or how to shape the edge.cut layer. After I finished the initial design, I realized, "I can’t actually fit this PCB inside a 1590B enclosure, even with the audio jacks and DC jacks.
So, I decided to redesign it from scratch.

<p align="center">
  <img src=asset/final_ver_pcb.png width="30%" height="30%">
</p>
After several revisions, this layout was finally completed.

I needed to go through multiple revisions because I wasn’t used to placing footprints efficiently at first. So I practiced by redesigning the PCB several times.

You might also notice there’s a hole in the middle of the PCB. When I finished the last revision, I realized I hadn’t considered the position of the DC jack. Instead of redesigning the PCB again, I came up with the idea of making a hole in the middle.

As far as I know, there’s no guitar effect pedal that places its DC jack in the center like this, so I was quite satisfied with that clever solution.

#### Enclosure consideration

While revising the PCB, I measured the actual size of the 1590B enclosure I had using vernier calipers. Then, I drew the footprint net to match the exact dimensions of the enclosure. I measured both the outer rectangle and the inner rectangle: the outer size was used for aligning the printed guide paper to the outside of the enclosure, while the inner size was used to set the board's dimensions and place internal components such as audio jacks and the foot switch.

I also added arrows to indicate where holes would be drilled for the LED and potentiometer shafts. Since these parts are visible from the outside, I carefully positioned them to match the enclosure’s proportions. Additionally, I made sure to leave enough distance between the shafts so that, once the knobs are attached, they won’t interfere with each other. 

As you can see on the bottom left, there is the enclosure footprint. As you can see on the bottom right, I drew the PCB layout directly over this footprint to make sure my design would fit perfectly inside the enclosure.
<p align="center">
  <img src=asset/pcb_case.png width="80%" height="80%">
</p>

## PCB Ordering & Assembly
### Ordering, cutting PCB
<p align="center">
  <img src=asset/pcb_cad.png width="50%" height="50%">
</p>
<p align="center">
  <img src=asset/pcb_cut.jpg width="50%" height="50%">
</p>
With the availability of low-cost PCB manufacturing services these days, I decided that outsourcing production would be far more efficient than attempting DIY etching. I placed an order through PCBWay, a popular PCB fabrication service.

To reduce costs, I took advantage of their fixed pricing for boards under 10cm x 10cm. I combined multiple pedal circuit designs into a single multi-layout board and omitted the V-cut option to stay within budget. I cut the board using a wire saw and smoothed the edges with sandpaper.
### Enclosure drilling
<p align="center">
  <img src=asset/drill.jpeg width="80%" height="80%">
</p>
To ensure precise drilling for the enclosure, I first printed the footprint at a 1:1 scale to create a drilling guide. I glued the printout onto the enclosure and drilled according to the marked holes.

To achieve accurate alignment, I followed these steps:

1. Punctured the center of each hole on the paper using an awl.
2. Used a nail to make a light center punch.
3. Drilled pilot holes with a 1.5mm bit.
4. Enlarged the holes using a step drill bit to the desired size.
### Soldering & Testing
<p align="center">
  <img src=asset/bare_test.jpg width="50%" height="50%">
</p>
I soldered the PCB as usual and tested it on a bare board. At first, it made a very loud, stinging noise when the gain knob was turned up high, and when the gain was lowered, it made a sound kind of like oil frying.

I suspected that when I was soldering the JFET — the one before the volume pot — I might have accidentally created a solder bridge between the JFET pins. I removed the bridge immediately at that moment, but when I tested the circuit later, the problem still appeared. 

So I desoldered the pads and tested again, but it didn’t fix the issue. At that point, I thought the pads on the PCB might have been damaged during the work.Or, JFET had been damaged by short. Then, I removed the JFET from the pads completely and tested if it is still working, and hardwired it directly into the circuit. As it turns out, the JFET was working fine; the problem was with the pads. You can see this hardwired setup in the picture below.


<p align="center">
  <img src=asset/hardwire.jpeg width="30%" height="30%">
</p>

### Debugging
After fixing the JFET issue, there was still a high-frequency oscillation in the treble range when the gain was turned up. I had no idea how to deal with it at first, so I discussed it with GPT. GPT helped me identify differences in the TL072’s higher slew rate as possible causes. However, as I didn’t want to edit and reorder the PCB, I couldn't change TL072 with LM308. So I decided to keep using the TL072.

Then I figured out there was another chip, the JRC4558, which has the same pinout but a lower slew rate than the TL072. So I ordered a 4558 and replaced the TL072, but it still didn’t fix the oscillation.

After discussing it again with GPT, I realized the problem might be the ceramic disc capacitors. Although they were used in the original circuit without issues, the LM308 has a significantly lower slew rate (0.3 V/µs) compared to the TL072 (13 V/µs) or even the 4558 (1.7 V/µs). The reason slew rate matters is that a higher slew rate allows an op-amp to respond faster to changes in the signal, which can lead to unintended oscillation if the circuit isn’t properly compensated. Also, ceramic disc capacitors tend to have higher ESR and less stable characteristics at high frequencies. When this used as a feedback capacitor, this can prevent high frequencies from being properly bypassed, which keeps them in the feedback loop and leads to oscillation.

So I substituted the ceramic discs with MLCC capacitors, which have lower ESR at high frequencies. And, as you can see in the picture below, the ceramic disc capacitors have been replaced with MLCC types."
<p align="center">
  <img src=asset/mlcc.jpeg width="40%" height="40%">
</p>


It worked quite well — the oscillation threshold (i.e., the gain level at which oscillation starts) has increased. In other words, I can now turn the gain up higher without encountering oscillation.
Additionally, since I used a chip with a higher slew rate — which allows for a higher gain under the same conditions — I also changed the value of the gain potentiometer. The original 100kΩ pot caused excessive amplification even around the halfway point, so I replaced it with a 50kΩ pot.

---------------
<p align="center">
  <img src=asset/return1.png width="40%" height="40%">
</p>

However, I still noticed some oscillation when the gain knob was set above 50%. Below that point, the circuit shaped the guitar signal reasonably well, so I didn’t consider it a major issue — especially since this was an experimental design.

While organizing the project documentation, I revisited the original RAT2 schematic and realized something important: as shown in the image at the above, the original circuit includes a high-impedance return path at the input, effectively creating a high input impedance.

Upon further research, I discovered that this was meant to compensate for the relatively low input impedance of the LM308 (around 2 MΩ).
In contrast, op-amps like the 4558 (5–10 MΩ) or TL072 (up to 1 TΩ) have significantly higher input impedance, making the circuit much more sensitive — essentially turning it into a noise absorber and eventually causing oscillation. So, I modified the return path resistor, as shown in the image below. In the photo, you can see both the updated schematic and the temporarily soldered resistor that follows the new design.

<p align="center">
  <img src=asset/return2.png width="60%" height="60%">
</p>

Also, you may notice that this is a second, identical circuit board. I managed to solder the transistors perfectly on this one. As a result, I now have two boards: one with the original 2.2 MΩ return path, and another with the lowered return path — as shown in the image above.

I compared both boards in the same signal chain, even swapping their positions during testing. You can watch the comparison video below.

[![Video Label](http://img.youtube.com/vi/drDYGywl7zI/0.jpg)](https://youtu.be/drDYGywl7zI)

## Sound review
First, listen to the sound of the video below.


[![Video Label](http://img.youtube.com/vi/0dIAr1F7TB0/0.jpg)](https://youtu.be/0dIAr1F7TB0)



I played a short riff from “Holy Diver” by Dio using two pedals with the same circuit design, but each equipped with a different op-amp chip. The yellow one uses a JRC4558 chip, while the black one has a TL072 chip.
You might notice that the pedal with the 4558 sounds darker, with relatively smoother treble. In contrast, the TL072-equipped pedal sounds brighter, with slightly sharper treble.

In case you didn’t catch the difference, I analyzed their frequency responses using a tool called “Match EQ,” which shows the average frequency response as an FFT graph. You can see the results in the picture below: the yellow graph represents the 4558, while the blue graph shows the TL072. I also pointed out the differences across various frequency bands in the analysis.
<p align="center">
  <img src=asset/fr_analyze.png width="80%" height="80%">
</p>
These variations match the typical characteristics of each chip: the JRC4558 has a relatively lower slew rate, which contributes to its darker and smoother tone, whereas the TL072 has a higher slew rate, resulting in a brighter and slightly sharper sound.

## Conclusion
Changing the RAT2’s op-amp chip was quite a reckless move. Especially since the RAT2 circuit has only a single op-amp stage. Op-amps with a high slew rate, like the TL072, are rarely used in single-stage designs because they’re more prone to oscillation.

Honestly, I didn’t know that at first. But it turned out to be a great opportunity to learn how to handle such cases. On top of that, I ended up with a unique guitar pedal design that actually sounds pretty good. So although it wasn’t easy, it was very productive and rewarding. I’m really glad I successfully completed this project and built the pedal. 
Thank you for reading this far!
