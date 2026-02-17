---
layout: post
title:  "PCB Fundamentals 3: CMOS Logic"
date:   2025-12-13 05:30:21 +0200
categories: Senseo 
published: true
---
Talk about why there is such a thing as negative voltage for PMOS
Talk about why combining and NMOS and a PMOS leads to energy saving

We now come to the CMOS logic gates. We'll cover the main ones that we also know from school, but note that the Inverter truly is the building block for everything. Also later on, when moving into feedback and more complex subsystems of the MCU, this is the logic circuit that keeps coming back. 



https://chatgpt.com/share/6993fe33-f1b0-800b-a369-f9591c7473c9







#### Logic Gates

I’ve seen these before in classes but never connected them to anything, but there are three main logic gates: NOT, AND & OR. Each of these is just a couple transistors arranged so that their voltages affect each other in predictable ways. 

A **NOT gate** (called an inverter) gives the opposite of what it receives, if it gets 1, it outputs 0, and vice versa. 
....
These are all soldered onto one of the copper traces of the board. An output node is actually nothing more than the copper in between the legs of resistor and transistor, where we can attach a multimeter and measure the voltage. With the transistor, we have it so that one leg connects to the output node (copper trace coming in), one connects to the ground, and the last forms the input node. The fact is that this input node, the gate of the transistor (for MOSFET transistors), is soldered onto another copper trace of the board. 

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














This is how we get opposite switch behavior for these two types of MOSFET transistors. With CMOS logic then, the hole idea is of using the two transistors together in a way that has one pulling the output up when needed and the other down. The output, contrary to RTL where it was a node in between resistor and transistor, is now always either connected to VDD (logic 1 - high voltage) or GND (logic 0 - low voltage). 

**Inverter**

The  NOT gate or inverter switches the input. is actually extremely simple in how it is wired.. This is simply wired like 

VDD - PMOS - Output - NMOS - GND

This implies that PMOS and NMOS are both tied to the same input. If we have input 0, PMOS will be on (NMOS off) and output is connected to VDD (power). No current can flow from VDD to GND, so output will be high (1). Similarly when input is 1, NMOS is on, output is connected to ground and no current flows from VDD to GND (output 0).

Now for the AND & OR gates, it must be noted that CMOS actually doesn’t directly create these, but instead naturally forms NAND & NOR after which we position an inverter (similar to AND for RTL). 

**AND Gate**

Starting with NAND, the idea is that we now again have a pull-up and pull-down path but this time with four transistors in total. NAND is simply that output is 0 only if A and B are 0. 

VDD = PMOS (A) = PMOS (B) = Output - NMOS (A) - NMOS (B) - GND

Again, ‘=‘ refers to being parallel, PMOS A and B are both connected with one side to VDD and another to output. For the pull-down path (NMOS - on if input 1), we want to have a connection to ground only if A and B are 1, which is achieved by having two NMOS in series. If both are receiving high voltage at their gate, both are on and output is pulled to 0. If either A or B has input 0, no output will be high. The pull-up path (PMOS - on if input 0) then has the goal of connecting power to output when either A is 0 or B is 0. As such two PMOS are put in parallel so that if A has input 0, PMOS(A) is on and output is 1 and similar for B. Only when both A and B have input 1 are both PMOS off. So PMOS A and NMOS A are connected to the same input. In the end we have a situation where output is 0 only when both A and B have input 1. 

When we now connect this to a NOT gate, we get the opposite version, where output will be 1 only when both A and B have input 1. What’s done is that the output node of NAND is connected to the inputs of the inverter (both PMOS and NMOS). When output of NAND is high, NMOS on the inverter is turned on and there is a connection to ground (0). When output of NAND is 0 (meaning both A and B have input 1), the PMOS on the NOT gate will turn on while NMOS is off, and output of the inverter is also 1. It’s tricky but intuitive. 

**OR Gate** 

The NOR gate again works with the A & B logic, such that output of NOR is only 1 when both A and B have input 0. If either A or B has input 1, output becomes 0. The full path now looks like 

VDD - PMOS (A) - PMOS (B) - output = NMOS(A) = NMOS(B) = GND

Such that now PMOS connecting to VDD fall in series while NMOS is parallel with both A and B connecting to output and GND. On the NMOS side (pulling down to ground), the idea is that we get the output low (0) if either A or B has high input (1). Meanwhile with the PMOS side (pulling up to VDD), we allow output to be high (1) only if both A and B are on (1). 

If we now again connect the NOR output to a CMOS inverter, we flip it and get the usual **OR gate**.

There something to be said about why CMOS is superior to say RTL. Remember when discussing RTL NOT gates how, when input is high (current flowing to transistor), the transistor is on and current can flow from power to resistor to transistor and then to ground. As a result we have output 0, which is great as the logic works, but means there is a continuous wastage of power, creation of heat and a slower speed than with the CMOS. Meanwhile with CMOS we have that every logic level has one transistor off, resulting in there never being a path from VDD to GND (no static current except during dynamic switching). This power dissipation during dynamic switching is the reason my laptop gets hot while running code but cools off immediately when sitting idle. 
