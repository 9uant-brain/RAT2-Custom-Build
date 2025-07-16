
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
  <img src="https://github.com/user-attachments/assets/c6f62f10-a760-46af-982d-2febacfc35e3">
</p>

### Power supply section

This is the 9V voltage input section, and all other parts of the circuit receive their power from here. The diode conducts only when the voltage is reversed. When it conducts, all current flows through a 100-ohm resistor. This happens because, for reverse current, the resistance on that path is significantly lower. The 100uF capacitor is for 9V voltage stabilization, while the two 100k resistors form a voltage divider that converts the 9V supply to 4.5V. The 1uF capacitor then stabilizes the voltage from this voltage divider.

### Clipping (gain) section 
This is core section. The circuit uses an LM308 to form a non-inverting amplifier, for amplifying raw guitar signal. At the op-amp's output, there's a back-to-back diode clipping circuit. Signals exceeding the diodes' forward voltage are shunted to ground, which clips the upper part of the waveform. This is the principle behind the characteristic "chugging" sound of electric guitars. So, changing diodes is a common modification for distortion and overdrive pedals among guitarists.

### Tone, output section
Following the back-to-back clipping stage, you'll find the tone knob (potentiometer). This component adjusts the resistance (R) in an RC filter to control the high frequencies of the output signal. After this, a JFET (5458) acts as a source follower, serving to adequately boost the signal's strength.

### LED Control section
You might know this circuit as 'The Millennium Bypass.' Usually, you'd need a 3PDT switch for a true bypass with an LED indicator control. But back in the day, 3PDT switches were hard to find. That's why pedals like the RAT used this bypass method. Normally, a 2PDT switch only gives you true bypass. However, by adding a JFET, this design also lets you control the LED's on/off state.

Below, I've sketched out simple schematics for three different true bypass methods, all in their bypass mode. Most of these are pretty straightforward, but the Millennium Bypass has a counter-intuitive quirk with its LED section: the LED actually goes off when LED section is connected to the circuit. I'll explain exactly why that happens further down.
<p align="center">
  <img src="https://private-user-images.githubusercontent.com/204548792/466495233-fb15be54-6533-483f-9528-173ae62bb91f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTI1ODQ3NzgsIm5iZiI6MTc1MjU4NDQ3OCwicGF0aCI6Ii8yMDQ1NDg3OTIvNDY2NDk1MjMzLWZiMTViZTU0LTY1MzMtNDgzZi05NTI4LTE3M2FlNjJiYjkxZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNzE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDcxNVQxMzAxMThaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iOWExNjAwZTBmMDZhZDlmYmNhMGRiZmExMjNkMzIyNWUwNDNjOGRlYzM4MjRjMGRkNzVjNzUxODA4Y2FiZjliJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.k9JkBHPuEVCBAlt5nwCOrKCi7fEsQVsaK0DBf9QoU9c">
</p>

### The Millennium Bypass

<p align="center">
  <img src="https://private-user-images.githubusercontent.com/204548792/466499846-83423f2c-1d2c-466c-af22-9954b89b2693.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTI1ODUwNTEsIm5iZiI6MTc1MjU4NDc1MSwicGF0aCI6Ii8yMDQ1NDg3OTIvNDY2NDk5ODQ2LTgzNDIzZjJjLTFkMmMtNDY2Yy1hZjIyLTk5NTRiODliMjY5My5naWY_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNzE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDcxNVQxMzA1NTFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jYjM5ZGIyNDhjZDE2ODY5OWExNjIzMTE2NTcyOGUzYzQxZjU3MGQxN2Y5MjZhNzIxN2I1OTFhMzdkNjA3MmNiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.djlTcDFREDv_DD06N_J3L01eSS76F3ktSGbd0yrHiTI" width="50%" height="50%">
</p>
As you can see in the picture on the above, when the circuit is in bypass mode, the gate of the 5458 JFET is pulled directly to GND(Lime line). Because the gate is at GND, Vgs becomes almost 0, so the JFET channel closes. Even though 9V is still connected through the 2M2 resistor, its high resistance prevents it from affecting the gate voltage significantly.

On the other hand, when the effect is engage mode(the lime route is disconnected), the gate is only pulled up to 9V through the 2M2 resistor(Red line). This creates a sufficiently positive Vgs, allowing the JFET channel to open.

### Schematic Creation
	
 <p align="center">
  <img src="https://private-user-images.githubusercontent.com/204548792/466495237-f4642b37-b8d7-4040-9003-0a6b1cbda21f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTI1ODUxMDEsIm5iZiI6MTc1MjU4NDgwMSwicGF0aCI6Ii8yMDQ1NDg3OTIvNDY2NDk1MjM3LWY0NjQyYjM3LWI4ZDctNDA0MC05MDAzLTBhNmIxY2JkYTIxZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNzE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDcxNVQxMzA2NDFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xYzBkYjBiYzgzOTRhMWYxMWEwMzhhNTBmYTBiYTg4NTZiNzk4ZTY5YWYzYTMyYzYxMmMwYmU1ZGZkYjU2ODA1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.YMrOmXduQZvBCqsLPTaM9H2DFLwhM4kcK39OkhZe6S4">
</p>
While the design largely follows the original, I encountered some resistance values that weren't available in my inventory. For these, I used series or parallel combinations to achieve approximate values. 
The original RAT 2 uses the LM308 op-amp, but I wasn't keen on using a chip so specifically limited to just this pedal. Therefore, I opted for the TL072, which is more commonly used and versatile in guitar effects. 
Since the LM308 is a single op-amp and the TL072 is a dual op-amp, I adjusted the pinouts accordingly. I also configured the circuit to deactivate the second op-amp of the TL072.

### PCB layout Creation

<p align="center">
  <img src=" https://private-user-images.githubusercontent.com/204548792/466495235-8c4abffb-5004-4ab0-b22b-dad97e9048ef.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTI1ODUxMDEsIm5iZiI6MTc1MjU4NDgwMSwicGF0aCI6Ii8yMDQ1NDg3OTIvNDY2NDk1MjM1LThjNGFiZmZiLTUwMDQtNGFiMC1iMjJiLWRhZDk3ZTkwNDhlZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNzE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDcxNVQxMzA2NDFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04OGNmNTY1YmI5NWM1Zjk5OTdiNTUxMzA0N2NhMTMwMTAzYjJiMDkyYjI4YWI3MjBjZmQ1ZmM3N2RmOTQ1ZWZiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.uL-wr8S3YLeoLNmfoLzBv3_OTec35mz4EIwONt3SqCs
" width="50%" height="50%" >
</p>



The picture above shows an early version of the PCB layout. 
Since I had to learn PCB design on my own, at first I wasn’t sure how large to make the board or how to shape the edge.cut layer. After I finished the initial design, I realized, "I can’t actually fit this PCB inside a 1590B enclosure, even with the audio jacks and DC jacks.
So, I decided to redesign it from scratch.

<p align="center">
  <img src="https://private-user-images.githubusercontent.com/204548792/466495239-957cd9c8-42e4-4bc4-8edd-c0a3161ffdbe.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTI1ODUxMDEsIm5iZiI6MTc1MjU4NDgwMSwicGF0aCI6Ii8yMDQ1NDg3OTIvNDY2NDk1MjM5LTk1N2NkOWM4LTQyZTQtNGJjNC04ZWRkLWMwYTMxNjFmZmRiZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNzE1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDcxNVQxMzA2NDFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00MmNkOTY4NGQwZjdjNThhNjEwMDhkOTFkN2QwYWRkZTNkMmI2ZjE4ZDJmOTk3NzQ2YzA2Zjk1OTBhNjkzMWVjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Uto47D-13c9zDUpiVAIIcj4gLhxT1OiMZU5BUsus8IU"  >
</p>
After several revisions, this layout was finally completed.

I needed to go through multiple revisions because I wasn’t used to placing footprints efficiently at first. So I practiced by redesigning the PCB several times.

You might also notice there’s a hole in the middle of the PCB. When I finished the last revision, I realized I hadn’t considered the position of the DC jack. Instead of redesigning the PCB again, I came up with the idea of making a hole in the middle.

As far as I know, there’s no guitar effect pedal that places its DC jack in the center like this, so I was quite satisfied with that clever solution.
