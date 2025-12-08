---
layout: post
title:  "PCB Fundamentals 2: Transistors & Logic Gates"
date:   2025-12-08 05:43:21 +0200
categories: Senseo 
published: true
---
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
