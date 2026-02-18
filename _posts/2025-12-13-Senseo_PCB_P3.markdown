---
layout: post
title:  "PCB Fundamentals 3: CMOS Logic"
date:   2025-12-13 05:30:21 +0200
categories: Senseo 
published: true
---
## CMOS

We've now seen the most important building block for pretty much all the electronics in our Senseo, so the next step is to explore how it's utilized to be as effective as it is. We're therefore in the world of CMOS logic, and still operate inside the brain of the machine, the MCU. 

CMOS stands for Complementary MOS (metal oxide semiconductor). But what does complementary mean in this context? Well as we'll see more formally for the inverter, in every logic gate we use, we implement both PMOS and NMOS. These work in opposite direction, gate (input) being high turns on the one while turning off the other. We could then have a series where we have a power source which connects to one of these transistor, and that one is then connected to another transistor, of which the far end is tied to ground. This leads to a situation where only one side conducts at a time, because if input is high (or low) only one of those two transistors will conduct.

The middle point, where the two transistors connect with each other, can then be used as an output node. This is because when we have our input going into this 'circuit', the output will be either high (1, as the power conducts) or low (0, as ground is conducting). 

This is why CMOS is so preferred in practice. The combination of both PMOS and NMOS turn logic circuits into a very low static power consumption. Power is never (except very briefly during switching) allowed to be wasted and go from the power rail to ground. It's energy efficient. 

## Inverter - NOT gate

The inverter is actually the building block for most other logic gates and truly lies at the basis of even the more complex electronic components of the MCU we'll see later on. The idea is that if we have an input of say 0, the output will be 1, and vice versa. If we now fomalize the above explanation of a little circuit with two different transistors, we arrive at the following:

VDD - PMOS -output- NMOS - GND 

As we saw before, VDD stands for voltage at the drain (Voltage Drain Drain) but in modern contexts we just see it as the voltage from the power supply. 

GND is ground, but note that this isn't really a connection to the earth/ground pin in the wall socket. Voltage only works in terms of reference points. If we say something is +5V, it is only so relative to a certain reference point. We saw this in batteries, and also in the consideration of the MOSFET. It's the same idea here. Ground on this logic gate is merely a point we choose as a reference. It's another node of the power supply. You can view this like the use of sea level as the reference point where we say height is 0. 

Our PMOS transistor is tied to the power rail, and conceptualizes the pull-up path. On the other side, we then have NMOS tied to ground. In the middle of the two, where the pins of PMOS and NMOS connect, is the output node. It's essentially just the place we could connect a multimeter and measure the voltage. 

Now the reason why we put PMOS against power and NMOS against ground, and not the other way around, becomes quite intuitive if we consider the way each of these works. Remember that PMOS only turns on if Vgs < 0, meaning that voltage at the gate is smaller than voltage at the source terminal. It then becomes convenient that a power supply puts the source terminal at a constant higher voltage. So when input is low (gate 0) the source is necessarily higher, and the PMOS conducts. These is the situation that naturally leads to 'negative' voltage, and subsequently to the electrons being repelled from the surface and holes being attracted - having the path (little bridge) formed. 

Similarly, NMOS turns on if Vgs > 0. With NMOS against ground, source is always low. And so when input is high (gate 1), voltage at the gate is necessarily higher than voltage at the source terminal. And so the NMOS conducts. This is called the pull-down path, as it pulls down the output to ground whenever NMOS conducts. 

Do note that the gates of both NMOS and PMOS are tied to the same input. There's actually an external little path between the gates in order to make it more efficient and just have the input wire connect to on input node (call it X). 

<div style="text-align: center;">
  <img src="/assets/SenseoViz/15/Inverter3D.JPG" width="340">
</div>

Now all put together, we can consider two states: 
- Input high: When input is high, NMOS conducts while PMOS doesn't. So output will be pull down to ground. An input of 1 leads to an output of 0. 
- Input low: when input is low, we have PMOS conducting and NMOS not. As such, output will be pull up to VDD. An input of 0 leads to an output of 1. 
 
### Bits and Physical Voltages 
Something that might be worthwhile to point out is that voltage being high or low is denoted as the data signal, the binary digit (bit) being 1 or 0. In data language, there's no such thing as 0.8 or 0.33. Instead, we consider 0-0.8V as a 0 bit, and 2-3.3V (or 5V) as a 1 bit. The middle region is undefined in digital sense, thereby ensuring a sort of noise margin.

No physical object can actually sense these strict 0 and 1's. These are purely human convention and refer to low or high voltage. In software we use them, but in physical objects everything is continuous. When I refer to output or input being 1/0, do note then that this is my interpretation, and in reality it just means the voltage there is high or low. 

The inverter actually helps straighten out a jittery input, as it pulls the output either strongly to VDD or to GND. 

--> To expand, talk about when which starts turning on and why inverter straightens out the input 

https://chatgpt.com/share/69954cac-bb90-800b-ab55-8f601ec2d1f3


## AND Gate 

Now for the AND & OR gates, it must be noted that CMOS actually doesn’t directly create these, but instead naturally forms NAND & NOR after which we position an inverter. 

Starting with NAND, the idea is that we now again have a pull-up and pull-down path but this time with four transistors in total. NAND is simply that output is 0 only if A and B are 0. 

VDD = PMOS (A) = PMOS (B) = Output - NMOS (A) - NMOS (B) - GND

Again, ‘=‘ refers to being parallel, PMOS A and B are both connected with one side to VDD and another to output. For the pull-down path (NMOS - on if input 1), we want to have a connection to ground only if A and B are 1, which is achieved by having two NMOS in series. If both are receiving high voltage at their gate, both are on and output is pulled to 0. If either A or B has input 0, no output will be high. The pull-up path (PMOS - on if input 0) then has the goal of connecting power to output when either A is 0 or B is 0. As such two PMOS are put in parallel so that if A has input 0, PMOS(A) is on and output is 1 and similar for B. Only when both A and B have input 1 are both PMOS off. So PMOS A and NMOS A are connected to the same input. In the end we have a situation where output is 0 only when both A and B have input 1. 

When we now connect this to a NOT gate, we get the opposite version, where output will be 1 only when both A and B have input 1. What’s done is that the output node of NAND is connected to the inputs of the inverter (both PMOS and NMOS). When output of NAND is high, NMOS on the inverter is turned on and there is a connection to ground (0). When output of NAND is 0 (meaning both A and B have input 1), the PMOS on the NOT gate will turn on while NMOS is off, and output of the inverter is also 1. It’s tricky but intuitive. 

## OR gate 

The NOR gate again works with the A & B logic, such that output of NOR is only 1 when both A and B have input 0. If either A or B has input 1, output becomes 0. The full path now looks like 

VDD - PMOS (A) - PMOS (B) - output = NMOS(A) = NMOS(B) = GND

Such that now PMOS connecting to VDD fall in series while NMOS is parallel with both A and B connecting to output and GND. On the NMOS side (pulling down to ground), the idea is that we get the output low (0) if either A or B has high input (1). Meanwhile with the PMOS side (pulling up to VDD), we allow output to be high (1) only if both A and B are on (1). 

If we now again connect the NOR output to a CMOS inverter, we flip it and get the usual **OR gate**.
