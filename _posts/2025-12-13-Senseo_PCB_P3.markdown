---
layout: post
title:  "PCB Fundamentals 3: CMOS Logic"
date:   2025-12-13 05:30:21 +0200
categories: Senseo 
published: true
---
We’ve now seen the most basic version of logic gates, RTL. The main idea there was that transistors are turned ON when high input is applied to the gate, resulting in a connection between drain and source and current being conducted (let through). Resistors then form the base of creating the logic gates. The inefficiencies lie mainly in the use of current to switch which wastes power, not scaling well (need lots of resistors) and having a low noise margin (meaning small voltage fluctuations can be misinterpreted as logic levels).

Complementary MOS (CMOS) logic uses MOSFET transistors in a complementary fashion. We’ve used MOSFET’s quite a bit to explain things, as they’re modern and the logic is the same, but haven’t yet explored what they truly are. 

MOSFET stands for Metal Oxide Semiconductor Field Effect Transistor. We’ve seen [semiconductors]({% post_url 2025-11-25-Senseo_Boiler_P3 %}) before, they’re just special materials like silicon that allow conduction or insulation depending on conditions. Oxide is a very thin insulation layer, while a piece of metal acts as a control electrode. The word Field comes from the fact that an electric field (voltage) controls whether the transistors switches on or off. So it’s a long name but a straightforward one. 

<img src="/assets/SenseoViz/13/MOSFETsymbol.png"
    width="230"
    align="right"
    style="margin: 0 0 1em 1em;">
As previously mentioned, such a MOSFET transistor has three main terminals: Source is where the flow comes in, Drain where the flow goes out (can be reversed) and Gate is the control deciding whether the flow can happen. This gate is the part that is insulated from current (flow), so it can only act as a control switch. This separation occurs because of that very thin insulation layer called oxide, which is actually Silicon dioxide (Silica - SiO₂) which we saw when [exploring glass]({% post_url 2024-10-02-Glass %}). So the Gate is actually a metal plate that is separated from the drain and source by an oxide (sort of glass) and then silicon. On the right we see how such a transistor is shown symbolically, we may then imagine the oxide to reside in that empty space between G’s metal and the drain and source parts. 

So being that gate is a metal plate, the application of voltage to it will make the electric field pass through, but the electrons cannot (due to the nanoscale glass insulation - nanoscale referring to 1-100nm, human hair is 80.000nm). Current is blocked by the Oxide, but the electric field passes through and pushes or pulls charges inside the silicon below. This silicon is the semiconductor, meaning that depending on this push or pull because of the electric field, it will be a channel that opens or closes.

<div class="context-box" markdown="1">
Small aside on Silicon: This semiconductor is much used in electronics, and is actually also made of sand (Silica, SiO₂). They start with very pure silica sand, which is mixed with carbon (e.g., wood chips or coal) and heated in an electric furnace at 2000 °C, causing the following reaction: SiO2 +2C→Si+2CO. 

Essentially the silicon element is separated and therefore purified (98% so, called metallurgic silicon). Then, something called the Siemens process is performed to convert this into even purer silicon. The metallurgic silicon is turned into a gas by reacting it with hydrogen chloride (Si+3HCl→SiHCl3 +H2). As such we obtain tri-chlorosilane (SiHCl3). This is then heated in order to get out the impurities (which all have different boiling points). Tall reactors then have U-shaped rods in them, made of polysilicon of prior runs having been melted/casted, and these rods are being electrified, creating heat (1000°C) through resistance (just like the boiler from before). This heating in the reactor decomposes the tri-chlorosilane gas we’ve made into a new solid silicon (SiHCl3 +H2 →Si (solid)+3HCl).

The (new) silicon is deposited onto the rods, making them thicker and [very pure polysilicon rods](https://en.wikipedia.org/wiki/Polycrystalline_silicon). These rods are as such placed in the reactor as a starting point. Apparently the resulting polysilicon rods are many small crystals fused together, while electronic chips require a perfect continuous crystal. Manufacturers therefore kind of ‘regrow’ the silicon, by for instance the Czochralski method. I’m eager to go down this path, but feel I would digress too much. The end result is just a giant single crystal silicon that can be sliced and used for electronics. That’s the semiconductor we find inside the MOSFET. 
</div>

Silicon contains two kinds of charge carriers, electrons (-) and holes (positive missing electron spots), very similar to the [diodes we saw before]({% post_url 2025-11-13-Senseo_Electricity_Part1 %}). Depending on how the silicon is chosen to be used, it will have mostly electrons (N-type) or mostly holes (P-type). We should imagine here that we have a bunch of people and chairs, and when everyone sits down, there are still chairs left open, those are the holes. So when electrons (humans sitting) move, the holes change places as well. Now the electric field communicating with the silicon can push negative charges and pull positive charges (holes).

There are two types of MOSFET, called PMOS and NMOS. As mentioned, silicon can be ‘doped’ in two ways, an the N-type then has a NMOS source and drain, while the P-type a PMOS source and drain. This is quite intuitive, when there are extra electrons there are additional negative charges, and vice versa. What we also already saw some time ago is that negative charges (electrons) are attracted to positive voltages, whereas positive charges (holes) are attracted to negative voltages. Positive or negative voltage here merely refers to reference point you’re at, whether you move upward or downward. This lies at the base of the difference between NMOS and PMOS behavior. 

We now know that with a MOSFET transistor the gate controls whether there is conduction (source and drain are connected). So given the different make-up of the two types, there is different conditions that will create conduction in both. For NMOS, the one with additional electrons in the silicon (semiconductor), there is a need for electrons to turn on the transistor as negative charges are attracted by positive voltage. A gate with high voltage (1) leads to NMOS being on (conducting). Similarly for PMOS, in order to have conduction, there is a need for holes so that the gate must attract holes in order to turn the transistor on. Positive charges are attracted by a lower (‘negative’) voltage, so when gate has low voltage the PMOS is ON. 

<img src="/assets/SenseoViz/13/NMOS.png"
    width="230"
    align="right"
    style="margin: 0 0 1em 1em;">
To see this more clearly, I found the picture on the right that signifies an NMOS type transistor. We can see this by the fact it has an N+ type source and drain, and a P-type body in between. When gate has high voltage, the electric field traveling trough the glass into the silicon, will attract the electrons present in the P-type body to the surface and form an N-type channel that connect the source to drain. 

It’s the exact same idea for PMOS, where drain and source are P+ and body N-type. When the gate has lower voltage than source, it means the gate is at much lower potential, so the electric field forms across the glass (negative voltage) and attracts holes - positive charges - to the silicon surface. This field slightly repels local electrons deeper into the N-type body, and actively attracts holes (positive charges) from the nearby P+ source and drain to form a P-type conduction channel. If both gate and source are on (eg 5V) than there is no electric potential difference, so no electric field is created and holes aren’t pulled to the surface. 

This is how we get opposite switch behavior for these two types of MOSFET transistors. With CMOS logic then, the hole idea is of using the two transistors together in a way that has one pulling the output up when needed and the other down. The output, contrary to RTL where it was a node in between resistor and transistor, is now always either connected to VDD (logic 1 - high voltage) or GND (logic 0 - low voltage). 

**Inverter**

We can start with the simple logic gate from which it all starts, the  NOT gate or inverted. This is simply wired like 

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
