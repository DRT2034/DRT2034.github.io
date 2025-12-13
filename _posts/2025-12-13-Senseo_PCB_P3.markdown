---
layout: post
title:  "PCB Fundamentals 3: CMOS Logic"
date:   2025-12-13 05:30:21 +0200
categories: Senseo 
published: true
---
We’ve now seen the most basic version of logic gates, RTL. The main idea there was that transistors are turned ON when high input is applied to the gate, resulting in a connection between drain and source and current being conducted (let through). Resistors then form the base of creating the logic gates. The inefficiencies lie mainly in the use of current to switch which wastes power, not scaling well (need lots of resistors) and having a low noise margin (meaning they easily turn ON).

Complementary MOS (CMOS) logic uses MOSFET transistors in a complementary fashion. We’ve used MOSFET’s quite a bit to explain things, as they’re modern and the logic is the same, but haven’t yet explored what they truly are. 

MOSFET stands for Metal Oxide Semiconductor Field Effect Transistor. We’ve seen semiconductors before [LINK HERE], they’re just special materials like silicon that allow conduction or insulation depending on conditions. Oxide is a very thin insulation layer, while a piece of metal acts as a control electrode. The word Field comes from the fact that an electric field (voltage) controls whether the transistors switches on or off. So it’s a long name but a straightforward one. 

<img src="/assets/SenseoViz/13/MOSFETsymbol.png"
    width="230"
    align="right"
    style="margin: 0 0 1em 1em;">
As previously mentioned, such a MOSFET transistor has three main terminals: Source is where the flow comes in, Drain where the flow goes out (can be reversed) and Gate is the control deciding whether the flow can happen. This gate is the part that is insulated from current (flow), so it can only act as a control switch. This separation occurs because of that very thin insulation layer called oxide, which is actually Silicon dioxide (Silica - SiO₂) which we saw when [exploring glass]({% post_url 2024-10-02-Glass %}). So the Gate is actually a metal plate that is separated from the drain and source by an oxide (sort of glass) and then silicon. On the right we see how such a transistor is shown symbolically, we may then imagine the oxide to reside in that empty space between G’s metal and the drain and source parts. 

So being that gate is a metal plate, the application of voltage to it will make the electric field pass through, but the electrons cannot (due to the nanoscale glass insulation - nanoscale referring to 1-100nm, human hair is 80.000nm). Current is blocked by the Oxide, but the electric field passes through and pushes or pulls charges inside the silicon below. This silicon is the semiconductor, meaning that depending on this push or pull because of the electric field, it will be a channel that opens or closes.



  Small aside on Silicon: This semiconductor is much used in electronics, and is actually also made of sand (Silica, SiO₂). They start with very pure silica sand, which is mixed with carbon (e.g., wood chips or coal) and heated in an electric furnace at 2000 °C, causing the following reaction: SiO2 +2C→Si+2CO. 

  Essentially the silicon element is separated and therefore purified (98% so, called metallurgic silicon). Then, something called the Siemens process is performed to convert this into even purer silicon. The metallurgic silicon is turned into a gas by reacting it with hydrogen chloride (Si+3HCl→SiHCl3 +H2). As such we obtain tri-chlorosilane (SiHCl3). This is then heated in order to get out the impurities (which all have different boiling points). Tall reactors then have U-shaped rods in them, made of polysilicon of prior runs having been melted/casted, and these rods are being electrified, creating heat (1000°C) through resistance (just like the boiler from before). This heating in the reactor decomposes the tri-chlorosilane gas we’ve made into a new solid silicon (SiHCl3 +H2 →Si (solid)+3HCl).

The (new) silicon is deposited onto the rods, making them thicker and very pure polysilicon rods [LINK TO GOOGLE IMAGES]. These rods are as such placed in the reactor as a starting point. Apparently the resulting polysilicon rods are many small crystals fused together, while electronic chips require a perfect continuous crystal. Manufacturers therefore kind of ‘regrow’ the silicon, by for instance the Czochralski method. I’m eager to go down this path, but feel I would digress too much. The end result is just a giant single crystal silicon that can be sliced and used for electronics. That’s the semiconductor we find inside the MOSFET. 


