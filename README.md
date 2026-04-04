# DIY-Vacuum-tube-amp-head

## Story
It's been a while since I noticed I don't have any audio or guitar amp I could use when I am at home. And when I was digging around some vacuum tube audio equipment, I saw that it is really expensive! It doesn't matter if it is single ended head or push pull (we don't even talk about these), the price starts at about $500 if you want something that will last and also something that have at least Overdrive (distortion) channel along with some tonal controls. That being said, I wanted to build my own amp and since I had and still have a lot of vacuum tubes at home, this seemed as easy one week project. At least I thought.

## Why Vacuum tube amplifier?
As some of you might know, Vacuum tubes produce often so called "warm" or "rich" tone. Most of all of us however like them more for their specific type of distortion, which differ from transistors quite a lot! From what I understand, audio amplification isn't always about perfection or 100% linearity. We are all humans, we aren't perfect and some of us (including me) prefer the non-perfect, nor clean and rather naturally distorted sound produced by these tubes.
- I am tired of using my transistor little preamp, that could drive barely 2W speaker
- I also wanted to build my own tube amp for a while, so I finally decided to make it! Keep reading, below is detailed guide of how I made it!



## Wanted features
- Powered by power bank
- Energy efficient design
- At least 5W of output power
- Low noise
- Sleek & minimalistic design
- Overdrive function
- Reliable & safe to work with

### Electrical features
- 4 ohm speaker output
- High Z (guitar) and Low Z (line level) inputs
- possibility to switch between these two
- Volume control
- Tone control
- OD switch

## Schematic & circuit
When it comes to schematic, I was inspired (as every usual vacuum tube builder do) by the Fender "CHAMP-AMP" guitar amplifier circuit from 50s. It is a really good design and a lot of tube amps come from this particular circuit I believe. Anyways, I designed my circuit using the [KiCad EDA](https://www.kicad.org/). It is really good software and I recommend it to everyone who is starting with schematic and PCB design.
- Below we will discuss every part of the schematic I made and I will explain what every component do

### Inputs
<img width="537" height="471" alt="obrazek" src="https://github.com/user-attachments/assets/37baa5ce-7b3e-4312-bc74-2b956f17a2f1" />

Our signal's journey starts at the input stage. By using SW2 we are selecting if we want the High impedance (guitar) input at the top, or the Low impedance (line) input at the bottom.
When we select the High Z input, the singal goes through the C2 capacitor acting as AC coupling device. The singal then goes through the SW2 and then there it the R1 which is used to set the input impedance of the U3 (ECC83) tube, to prevent unvanted noise and to set the impedance to 1M ohm so the input is really sensitive, which we want in case of weak guitar pickup signal. We don't want to pull it down through some low impedance.
When we select the Low Z input, the signal goes through R2 and R3 forming a passive mixer circuit, which converts our (expected) stereo input to mono at total impedance of roughly 500Ω. This signal then goes through the C1 which in this case is 100nF because if it was only 22nF, it would form high pass filter, efectively blocking our basses and letting through only mids and highs. After this cap, there is again R5 setting our impedance to lower value. Because line level signal is already preamplified signal, we don't need that much of a sensitivity on the ECC83's input.

- One more thing to mention, the reason I've put the 1M Ω resistor after the switch is because when we are switching to another input, there is a short period during the change of the poles when there is no contact to either one of the inputs. If we haven't had the resistor after the switch, we would put the tube's grid to practically infinite impedance and this is not good thing to do. Especially if you want the amp to work reliably and last "forever"
- And yes, I know I should've added some grid stopping resistors in the singals path, but I found out that this approach works the best among all possibilities.
- Also keep an eye when buying Single pole Dual throw (SPDT) switches, I had to modify mine because when I firstly put the amp together, they had practically zero contact in both of the positions

### Preamplifier & Power amplifier stage
<img width="812" height="476" alt="obrazek" src="https://github.com/user-attachments/assets/a26b5b4f-b7fe-4291-b72d-61628bf74eed" />

Our signal now continues onto first grid of U3 (ECC83) tube. R6 and C11 is cathode network that ensures the cathode is roughly 6V above ground. This is because we need to have the grid to be slightly negative in reference to cathode, beacuse otherwise the grid will just have zero effect on the electrons flowing from cathode to anode. R4 is anode loading resistor witch ensures the ECC83 has needed voltage and allows for the amplifications itself (it allows the ECC83 and any other tube where it is used to swing its voltage up and down without affecting the supply voltage). This preamplified signal then goes form first anode through the C2 capling cap to potentiometer RV1 which is out volume control and then it goes through RV2 pot with C8 forming basic low pass (high cut) filter that works as tonal control. The signal then goes to second grid of the U3 where it gets amplified again. Switch SW3 is clean/distortion select, but in this case it doesn't have much effect on the distortion itself. While tinkering with it, I found out that it rather changes the tone's character. While the 200k resistor gives more warm thin sound, the 100k gives more rich thick or wide sound. What it actually does is that it sets the tube's "swing headroom". For higher resistances (e.g. 200k and more) the tube has more swing headroom which results in less gain, thinner sound, but lower distortion. For lower resistances (100k and less) it results in higher gain, richer sound but it also has less headroom, so the sound starts clipping sooner and there is more distorion. After this amplification, the sound then goes through C9 (AC coupling cap, again...) and  the R11 sets input impedance on the 1st grid of the U1 (EL84) power tube. R10 and C10 forms again cathode network that raises voltage on cathode in refence to ground to about 6V. 2nd grid is connected to anode voltage through at least 1k resistor (R12). Normally you would connect it straight to anode voltage, but when the voltage on the anode swings, sometimes it swing so low the 2nd grid has much higher voltage than the anode. This would pull huge amount of current through the grid 2 which is normally rated to ten times less current than the anode. Long story short, you would soon destroy your power tube, so there is the R12 (in our case) to stop this tremendous amount of current.
