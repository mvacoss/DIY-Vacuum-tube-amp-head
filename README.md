# DIY-Vacuum-tube-amp-head

## Overview
I've designed this Vacuum tube amplifier head mainly because I wanted learn some interesting stuff about tubes. It is a dual tube compact class A amplifier powered from power bank or 12V DC adapter so I can carry it with myself without need of power grid. It consists of ECC83 double triode as preamplifier and EL84 power pentode as power amplifier.

It has a lot of useful features, including High Z and line-level inputs and clean and OD modes. It combines wood housing and aluminum sides along with some black knobs that makes it look somewhat reasonable. This project was part of my exhibition during SOČ day in our School. I've managed to complete it in three weeks and I think it turned out really well! The sound is just amazing!

<img src="https://github.com/user-attachments/assets/b899b94e-9ab4-4f7d-b91c-02d4a5569d63" alt="preview" style="width:30%; height:auto;">

## Main features
- Powered by power bank
- Energy efficient design
- More than 5W of output power
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

## Story
It's been a while since I noticed I don't have any audio or guitar amp I could use when I am at home. And when I was digging around some vacuum tube audio equipment, I saw that it is really expensive! It doesn't matter if it is single ended head or push pull (we don't even talk about these), the price starts at about $500 if you want something that will last and also something that have at least Overdrive (distortion) channel along with some tonal controls. That being said, I wanted to build my own amp and since I had and still have a lot of vacuum tubes at home, this seemed as easy one week project. At least I thought.

## Why Another vacuum tube amplifier design?
Well, it is simple. I wanted to learn about tubes and wanted to build something myself out of scratch
- I am tired of using my little transistor preamp, that could drive barely 2W speaker
- I also wanted to build my own tube amp for a while, so I finally decided to make it! Keep reading, below is detailed guide of how I made it!

As some of you might know, Vacuum tubes produce often so called "warm" or "rich" tone. Most of all of us however like them more for their specific type of distortion, which differ from transistors quite a lot! From what I understand, audio amplification isn't always about perfection or 100% linearity. We are all humans, we aren't perfect and some of us (including me) prefer the non-perfect, nor clean and rather naturally distorted sound produced by these tubes.

## Schematic & circuit
When it comes to schematic, I was inspired (as every usual vacuum tube builder do) by the Fender "CHAMP-AMP" guitar amplifier circuit from 50s. It is a really good design and a lot of tube amps come from this particular circuit I believe. Anyways, I designed my circuit using the [KiCad EDA](https://www.kicad.org/). It is really good software and I recommend it to everyone who is starting with schematic and PCB design.

Below we will discuss every part of the schematic I made and I will explain what every component do

### Inputs

<img width=40% height=auto alt="obrazek" src="https://github.com/user-attachments/assets/37baa5ce-7b3e-4312-bc74-2b956f17a2f1" />


**Input Stage & Impedance Selection (SW2)**

The circuit begins with a signal source selector (SW2) that optimizes the input for either a high impedance instrument (guitar or turntable output) or a line-level (low impedance) device. This is especially needed, when ou want to plug in output of your phone through some 3.5mm jack and don't want to have your signal "mushy" or without highs.

**High-Z (Guitar) Path**

When set to High-Z, the signal passes through coupling capacitor C2 to the grid of U3. R1 sets a high input impedance of 1MΩ.
Why is that?: This prevents "loading down" weak guitar pickups, preserving high-frequency detail and ensuring maximum sensitivity for the tube's first gain stage.

**Low-Z (Line) Path**

When switched to Low-Z, the signal goes firstly through passive mixing network consisting of R2 and R3. This converts stereo signal to mono signal with equvalent series impedance of roughly 500Ω. The coupling capacitor C1 is increased to 100nF in this case. A standard 22nF cap would interact with the lower impedance to create an accidental high-pass filter; 100nF ensures full-range bass response.
Sensitivity: R5 provides a lower grid leak resistance. Since line-level signals are already pre-amplified, the circuit requires less sensitivity and benefits from the added noise rejection of a lower impedance path.

- One more thing to mention, the reason I've put the 1MΩ resistor after the switch is because when we are switching to another input, there is a short period during the change of the poles when there is no contact to either one of the inputs. If we haven't had the resistor after the switch, we would put the tube's grid to practically infinite impedance and this is not good thing to do. Especially if you want the amp to work reliably and last "forever"
- And yes, I know I should've added some grid stopping resistors in the singals path, but I found out that this approach works the best among all of the possibilities.
- Also keep an eye when buying Single pole Dual throw (SPDT) switches, I had to modify mine because when I firstly put the amp together, they had practically zero contact in both of the positions


### Preamplifier & Power amplifier stage

<img width=50% height=auto alt="obrazek" src="https://github.com/user-attachments/assets/a26b5b4f-b7fe-4291-b72d-61628bf74eed" />

**Preamp stage: ECC83 (U3)**

The signal enters the first grid of U3. The cathode network (R6/C11) biases the cathode to +6V, creating the necessary negative grid-to-cathode relationship for electron control. The anode load resistor (R4) allows the tube to swing its voltage, translating current changes into an amplified signal.
This amplified signal passes through coupling capacitor C2 to the Volume (RV1) and Tone (RV2/C8) controls. The Tone circuit acts as a simple high-cut filter before the signal hits the second stage of U3 for further amplification.

**The "Tone character" switch (SW3)**

SW3 toggles the anode resistance to alter the tube's "swing headroom" and gain structure:
- 100kΩ Setting: Provides higher gain and a richer, "thicker" sound. With less headroom, the signal clips earlier, introducing more natural distortion.
- 200kΩ Setting: Increases headroom and reduces gain, resulting in a cleaner, "thinner," and warmer tonal character.

**Power amp stage: EL84 (U1)**

After the second preamp stage, the signal is AC-coupled via C9 to the power tube. R11 sets the input impedance for the EL84 grid. Similar to the preamp, R10/C10 form a cathode bias network, lifting the cathode to +6V.
- **Note on Screen Protection**: The screen grid (G2) is connected via R12 (1kΩ). While G2 is often tied directly to B+, the anode voltage can swing below the screen voltage during high output. Without R12 to limit current, the screen would draw excessive power and eventually destroy the tube.

**EL84 Calculations**

Desired currend: 40mA
- Rc = 6V / 0.04A = 150Ω
- Ua = 200V - (172.3Ω * 0.04A) = 193.1V
- Rg2 = at least 1kΩ
- [datasheet](https://www.tube-data.com/sheets/183/e/EL84.pdf)


### Output transformer & audio output stage

<img width=30% height=auto alt="obrazek" src="https://github.com/user-attachments/assets/4b6df414-b604-4db9-841e-6ba850a388a7" />

The power signal now goes on the start of the primary winding of T1 audio transformer. The end goes straight to the +200V. This ensures every difference in changing voltage on EL84's anode in reference to the +200V gets transformed to the 4Ω output which is then directly connected to J2 6.3mm output jack and to banana plugs.

**Output transformer**

I've got my output transformer from old tube radio. It had no secondary when I pulled it out however, so I had to go through really painful process of winding the secondary myself. At the end of the day, it has roughly 4Ω impedance and below I included some measured values in case you wonder.
- Primary  Z = 5.8kΩ  R = 172.3Ω
- Secondary  Z = 4Ω  R = 0.7Ω
Note: Z is the actual impedance of the transformer winding, while R is the DC resistance

### Power supply
<img width=38% height=auto alt="obrazek" src="https://github.com/user-attachments/assets/fe2f7a0c-0478-4d84-a079-5fd796510959" />
<img width=41% height=auto alt="obrazek" src="https://github.com/user-attachments/assets/04f017fe-f794-4fd5-a465-d0371550b8ac" />


For the PSU's I decided to go with buck and boost converters

**Filament voltage PSU (on the left)**: I went with HW-187 miniature buck converter. It steps down 12V on the input to rouhly 6.3V needed for the filaments. I also attached a proper heatsing along with thermal paste to ensure proper cooling.

**HV PSU (on the right)**: This one is a little more complicated. Firstly I went with the MAX1771 boost converter, but I accidentally blew it up and os it can only suply rouhly 2mA of current at really unstable voltage. So I am temporarily using the GS00066 step up with a bunch of caps and filtering, because this module is a lot of pain to work with. Especially if you want to do some audio stuff with it. Anyway, I power this monster temporarily from 4S LiPo battery.

I will be upgrading this as soon as new parts arrieve but until then, there is not much I can do.

And of course both of these are switching at frequency well above audible range.
