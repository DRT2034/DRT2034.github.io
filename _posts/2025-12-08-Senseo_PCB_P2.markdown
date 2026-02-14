---
layout: post
title:  "PCB Fundamentals 2: Transistors & Logic Gates"
date:   2025-12-08 05:43:21 +0200
categories: Senseo 
published: true
---

<img src="/assets/SenseoViz/11/TransistorMOSFET.png"
    width="230"
    align="right"
    style="margin: 0 0 1em 1em;">
I mentioned that **MOSFET transistors** are the reason this chip can achieve so much yet be so small. Now a MOSFET is like a tiny switch. It has three legs where drain and source connect two parts and gate is the pin that controls whether drain and source are connected inside. Next chapter we'll see this in more detail, but it's already nice to understand conceptually that we still talk about 0 and 1 here. Depending on the type of MOSFET (take NMOS as the example here), gate being pulled high (1) will create a channel inside between the drain and source and allow current to flow. 

With "current flowing" I don't necessarily mean electrons flowing like a river, but instead I mean that the electromagnetic wave causing voltage changes.

You might now be thinking, if the MOSFET is this chunky and big as we see here in the visual, how can up to a million fit inside the small MCU from before? Well MOSFETs come in various sizes. What we see in the visual is called a *power MOSFET*, which is what's used for handling big components on the PCB such as switching motors, heaters and pumps. In order to do this it needs to withstand tens to hundreds of volts, and so needs a physically larger silicon die, thicker metal layers and generally more robustness inside. Note that this type isn't used on our Senseo PCB, what you instead see in the first visual of last chapter in the middle of the board, is called a Triac. This is more suitable for bridging DC electronics with AC applications such as the boiler and pump, but more on this later. 

On the other hand, we then also have what's called *logic MOSFETs* (CMOS transistors). These are the tiny switches inside the MCU, which still range in size but on average are around 65nm (0.000000065 meters). Physically they work the exact same way. The reason these can be so much smaller is because they function in the world of DC (3.3-5V) and have no need for heat dissipation. These are build using photolithography and are responsible for much of the advances in tech today. As you may have gathered, and which will be discussed further on, silicon is an important component of these. In fact, silicon is the key material to make semiconductors, among which MOSFETs, and as such the name "silicon valley" suddenly gets meaning.

I also mentioned that "the microcontroller uses the transistors to switch the output pin to VDD (high voltage) or ground (low voltage)". An important clarification is that VDD stands for voltage at the drain. In logic circuits (modern CMOS), we'll see this come back many times. Other type of logic circuits do also exist, such as Bipolar Junction Transistor (BJT) circuits (which use Vcc, not Vdd), but since these are an older type that aren't in the Senseo nor in many modern appliances, we won't go into those. 

## Physical layout 

We've already mentioned this but it's word considering properly. A MOSFET transistor has three main terminals: **Drain** is where the flow comes in, **Source** where the flow goes out (can be reversed) and **Gate** is the control deciding whether the flow can happen. 

<img src="/assets/SenseoViz/13/MOSFETsymbol.png"
    width="230"
    align="left"
    style="margin: 0 0 1em 1em;">
This gate is the part that is insulated from the main pathway by tiny glass (oxide), so it can only act as a control switch and not receive or give actual current. This separation results from that very thin insulation layer called oxide, which is actually Silicon dioxide (Silica - SiO₂) which we saw when [exploring glass]({% post_url 2024-10-02-Glass %}). So the Gate is actually a metal plate that is separated from the drain and source by an oxide (sort of glass). Behind that oxide and in between the drain and source terminals resides the semiconducting silicon itself. In the visual we see how such a transistor is shown symbolically, we may then imagine the oxide to reside in that empty space between G’s metal (vertical full line) and the drain and source parts. 

Since gate is a metal plate, the application of voltage to it will make the electric field pass through, but the electrons themselves cannot. The nanoscale glass insulation that prevents this is about 1-100nm, while a human hair is 80.000nm. Current is blocked by this Oxide, but the electric field that passes through will push or pull charges inside the silicon below. This silicon is the semiconductor, meaning that depending on this push or pull because of the electric field, it will be a channel that opens or closes. We'll see this in more detail down below.

## Metal Oxide Semiconductor Field Effect Transistor

MOSFET itself stands for Metal Oxide Semiconductor Field Effect Transistor. That's a handful, but becomes quite straightforward once dissected. We’ve seen ['semiconductors']({% post_url 2025-11-25-Senseo_Boiler_P3 %}) before, they’re just special materials like silicon that allow conduction or insulation depending on conditions. 'Oxide' is a very thin insulation layer of silicon dioxide (SiO₂ - extremely thin glass) that sits between gate and the semiconductor (silicon). The word Field meanwhile comes from the fact that an electric field (voltage) controls whether the transistors switches on or off.  

This is then also where it gets interesting. On chemical note, Silicon (seen end of this chapter for an aside) contains two types of carriers: electrons (-) and holes (positive missing electron spots), very similar to the diodes we saw before. Depending on how the silicon is chosen to be used (called 'doped'), it will have mostly electrons (N-type) or mostly holes (P-type). We should imagine here that we have a bunch of people and chairs, and when everyone sits down, there are still chairs left open, those are the holes. So when electrons (humans sitting) move, the holes change places as well. Now the electric field communicating with the silicon can push negative charges and pull positive charges (holes). We can already see where the semi conducting behavior comes from. When an electric field is present, the make of the material changes. And the make of the material in this case are electrons. 

That's what leads us to two different types of MOSFETs, PMOS and NMOS.

## PMOS & NMOS 

Since silicon can be doped in multiple ways, we formally recognize two versions for our transistors. 

PMOS: In this case the silicon is P-type, meaning that the main charge carriers are holes. A hole is a missing electron that behaves like a Positive charge, so we can think of this material as having positive charge carriers.

NMOS: Here the silicon is N-type, where the main charge carriers are electrons. Electrons carry a Negative charge, so in this material the current is mainly carried by negative charge carriers.

As we discussed before, negative charges (electrons) are attracted by positive voltage while positive charges (holes) are attracte by negative voltage. A voltage being positive or negative merely depends on the reference point, so where we start from. 

We now know that with a MOSFET transistor the gate controls whether there is conduction (source and drain are connected). So given the different make-up of the two types, there is different conditions that will create conduction in both.
https://chatgpt.com/share/699002bf-6f3c-800b-86d4-5bbfca254813












<div class="context-box" markdown="1">
Small aside on Silicon: This semiconductor is much used in electronics, and is actually also made of sand (Silica, SiO₂). They start with very pure silica sand, which is mixed with carbon (e.g., wood chips or coal) and heated in an electric furnace at 2000 °C, causing the following reaction: SiO2 +2C→Si+2CO. 

Essentially the silicon element is separated and therefore purified (98% so, called metallurgic silicon). Then, something called the Siemens process is performed to convert this into even purer silicon. The metallurgic silicon is turned into a gas by reacting it with hydrogen chloride (Si+3HCl→SiHCl3 +H2). As such we obtain tri-chlorosilane (SiHCl3). This is then heated in order to get out the impurities (which all have different boiling points). Tall reactors then have U-shaped rods in them, made of polysilicon of prior runs having been melted/casted, and these rods are being electrified, creating heat (1000°C) through resistance (just like the boiler from before). This heating in the reactor decomposes the tri-chlorosilane gas we’ve made into a new solid silicon (SiHCl3 +H2 →Si (solid)+3HCl).

The (new) silicon is deposited onto the rods, making them thicker and [very pure polysilicon rods](https://en.wikipedia.org/wiki/Polycrystalline_silicon). These rods are as such placed in the reactor as a starting point. Apparently the resulting polysilicon rods are many small crystals fused together, while electronic chips require a perfect continuous crystal. Manufacturers therefore kind of ‘regrow’ the silicon, by for instance the Czochralski method. I’m eager to go down this path, but feel I would digress too much. The end result is just a giant single crystal silicon that can be sliced and used for electronics. That’s the semiconductor we find inside the MOSFET. 
</div>









#### Logic Gates

I’ve seen these before in classes but never connected them to anything, but there are three main logic gates: NOT, AND & OR. Each of these is just a couple transistors arranged so that their voltages affect each other in predictable ways. 

A **NOT gate** (called an inverter) gives the opposite of what it receives, if it gets 1, it outputs 0, and vice versa. What’s needed to built such a gate is one resistor and one transistor. These get arranged between the power supply (e.g. 5V) and ground (0V), and in between resistor and transistor we have the output node. That way we have:

5V - Resistor -(output node)- Transistor - 0V 

<img src="/assets/SenseoViz/11/TransistorMOSFET.png"
    width="230"
    align="right"
    style="margin: 0 0 1em 1em;">
These are all soldered onto one of the copper traces of the board. An output node is actually nothing more than the copper in between the legs of resistor and transistor, where we can attach a multimeter and measure the voltage. With the transistor, we have it so that one leg connects to the output node (copper trace coming in), one connects to the ground, and the last forms the input node. The fact is that this input node, the gate of the transistor (for MOSFET transistors), is soldered onto another copper trace of the board. Now when a resistor is between two points, it limits the current coming in, so that when nothing else draws current, the output node will eventually also rise to 5V. If there is a transistor conducting the current, it goes to ground (0V).

The transistor sits between output node and ground. The input signal (gate connected to separate copper trace) is then voltage coming from somewhere else (like another transistor, microcontroller pin, sensor etc.). If the input node is low voltage, the transistor is OFF, and vice versa. Now as mentioned before, whenever a threshold voltage is applied to the gate leg (usually starting at 1-3V), drain and source are connected (transistor is ON) and the transistor conducts current. 

As such, whenever the input node is low voltage, the gate sees 0V and the transistor is OFF. This way there is no connection between output and ground, and the resistor pulls the output node to 5V. Low input results in high output. If input voltage is high, transistor gets turned ON, creates a pathway and the output node gets connected to ground. The resistor is still trying to pull up the voltage on the output node, but the transistor is stronger. High input results in low output (inversion). The reason the resistor pulls up voltage when there is no connection to ground is because it limits current. A possible analogy here is connecting a thin straw (resistor) to a water tank, which limits flow but the bucket under the straw will still slowly fill. Now when something else tries to push voltage down (such as connection to ground), the resistor loses and output node is pulled to 0V. 

The **AND gate** on the other hand, outputs 1 only when all inputs are 1. It’s built using two or more transistors and one pull-up resistor (same as before), arranged so that either transistor can pull the output low. The idea is very similar to the NOT gate, in the sense that we have the same structure but with a second transistor stacked before the earlier one. The path is therefore:

Vcc (official name for power - 5V) - Resistor -(Output node)- Transistor A - Transistor B - Ground (0V). 

The transistors are connected in the same manner as before, and since they’re in series, transistor A being ON doesn’t lead to a path to 0V, this only happens when both transistors are ON. This way, both transistors ON (high input) leads to low output. This is called the **NAND** gate. In order to get from this to the AND gate, one more physical step is included. The NAND gate actually gets sent through the NOT gate, reversing the outcome of it. Having the former go through the latter makes it so that when the output of the NAND gate is high, the output of the NOT gate is low. 

Lastly, there’s also the **OR gate**. Similar to the AND gate, two transistors are used, but now the output is high when either of the two is high (ON). The structure is a bit different than that of the NAND gate, as it essentially gets reversed: 

Ground (0V) - Resistor -(Output node)- Transistor A = Transistor B - Vcc (5V)

The way I put the series may seem a bit strange, but the idea is that both transistors are connected with one of their legs to the output node and one to the 5V. So there’s no longer a series connection between the transistors. Both are used in parallel, so to speak, as they’re both connected to output node and 5V. The resistor is placed on the output node just before ground, and will pull down the voltage if either transistor allows it. If nothing influences the output node, its voltage will get pulled down to ground 0V. Now if either of the transistors provide a strong upward path to 5V, the output node will be connected directly to the 5V and output is high. This way, either transistor can create a path to 5V and only one needs to be ON to allow this. 

This part is quite interesting, actually. One might wonder what the use is of the resistor, since both transistors being OFF would lead to no current flowing through the output node anyway, right? Well apparently in that scenario, when both transistors are off, the output node becomes a floating one, and doesn’t automatically go to 0V. The output node is disconnected but is now only connected to its own being, resulting in unpredictable voltage. So while no current is flowing, the output voltage will be unpredictable and the logic gate doesn’t work as intended. If we’d then just connect the output node to ground via wire, that would also not work, since then the node would always be at ground (0V) regardless of the transistors. 

One must take care not to get confused when seeing these logic gates in the wild (google). It turns out that there are several transistor logic families, and the one we just explored here was the most basic version - Resistor Transistor Logic (RTL), where MOSFET transistors were implemented as the idea remains the same. The most common transistors used for the RTL logic are BJT (bipolar junction transistor) ones. Unfortunately RTL isn’t really used anymore, but is easy to understand. The modern version of a logic family is the CMOS, the Complementary Metal Oxide Semiconductor. CMOS then does actually use MOSFET transistors, which we'll explore next. 








#### Understanding components

I realize I just packed a lot of dense information in a couple of paragraphs, so let's go step by step and consider them thoroughly. 

As you likely noticed in the above picture, the **microcontroller** has 7 legs (pins) on each side. This is perfect size for having 8-10 GPIO pins, an ADC channel (temparature sensing), timer, PWM (pump) and an internal oscillator. All of which we'll see later on as well. Idea is just that these pins are the connection between the brain on the one hand, and on the other the PCB and appliance parts. When we press the power button, or 1/2 cup buttons, all this input is processed inside the MCU. Same goes for the temperature sensing. Meanwhile, the machine shows us LED lights that have a fixed meaning (blinking - brewing, fixed - ready) only because this tiny chip controls those lights. Both heater and pump do what they do, and when they do it, because the MCU directs them to. 

