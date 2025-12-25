---
layout: post
title:  "PCB Fundamentals 4: Combinatorial v Sequential Logic"
date:   2025-12-25 09:48:21 +0200
categories: Senseo 
published: true
---

The natural next step in our process of understanding is that of how to arrange voltages and use these logic gates to actually have decision making. We’d like to have a machine do what we’d like it to do when we push a button. Now with CMOS we already have a really powerful ingredient. We  can wire things in such a way that the output voltage is a function of input voltages. That’s the logic gates resulting from arranging PMOS and NMOS, which are essentially just doing Boolean algebra. 

**Combinatorial logic and Arithmetic circuits**

Combinatorial logic is now just a type of logic where the output only depends on the current combination of inputs. The idea is that if we were to freeze the inputs, the output would also be fixed. Output can only change if inputs change. This might seem straightforward, but there’s another type of logic that introduces the idea of memory and the notion of time. 

As a result, this type of logic is kind of like a calculator (arithmetic circuits), which adds, subtracts , compares, select and so on. Because CMOS gates are very reliable, and never show ambiguity about whether the output is high or low (because either VDD or GND is completely cut off), we can build bigger decisions out of smaller one. So really what is done here is mapping inputs to outputs, physically.

This mapping is actually then just a list of cases of which the AND, OR & NOT gates are the building blocks. Binary digits (bits) come from the fact that we implement thresholds for transistors to switch on/off, otherwise there would be major voltage noise. Through the use of these logic gates, these bits are turned into inputs and outputs, of which a truth table may describe the possible states. The next step towards arithmetic and specifically addition is then actually another type of logic gate: the **XOR gate**. Its importance comes from the fact that it can isolate just the differences.

With a regular OR gate, we have that output is high (1) if either A or B is on, *or both*. So the moment just one of them is one, output = 1. With XOR we just have that output is high only if A or B is 1, but not when both are high.  

Now if we want to add a couple of bits, which is the simplest case of hardware arithmetic, we’d get the following truth table: 

<div style="text-align: center;">
  <img src="/assets/SenseoViz/14/ADDtruth.png" width="340">
</div>

I had completely forgotten about the idea of ‘carry’, but it’s just the logic of kindergarten math: if we want to add for instance 156 and 155, we place them above each other and go number by number. Since 5 and 6 add to 11, we have the *carry* the one to the next addition of 5 and 5 (and then again to the next). The same logic comes in here. The sum column we can easily recognize as actually being just an XOR gate, while the carry is represented by an AND gate. The reason we have a carry here is because summing 1 and 1 makes 2, but hardware can only represent one bit per wire, so we carry the one. This carry then represents information not fitting the current bit position. 

Now these XOR and AND gates are merely the voltage classifiers we’ve seen earlier, but their joint output matches what we call addition. The hardware itself has no idea what it’s doing. 

Binary addition, such as in computers, produces two outputs out of inputs A and B: a sum bit (represented by XOR) and a carry bit (AND). These gates express the independent logical fact of the situation of A and B. These gates are then placed in a circuit so that we can have both facts (sum and carry) at the same time. Two wires go from A and B and in a parallel manner: 

<div style="text-align: center;">
  <img src="/assets/SenseoViz/14/HalfAdder.png" width="340">
</div>

This makes it a ‘half’ adder and essentially tries to answer two questions (see truth table), whether the voltages are different (XOR) and whether both voltages are high (AND). By having the wires to these gates in parallel, both can be answered and serve to decompose the relation of A with B. 

From this we can go to the **full adder**, because with binary addition when we add multi-bit numbers, each bit position must also accept a carry from the previous position. In a full adder, we then have the sum being the bit that stays in position, and the *carry out* which is the bit that moves to the next position. As such we have A + B + Carry-in —> (sum, carry-out). Remember the addition of 156 and 155, when we get to the sum of 5 and 5 in the middle, we also have a carry-in from before so must take this into account. 

<div style="text-align: center;">
  <img src="/assets/SenseoViz/14/FullAdder.png" width="340">
</div>

In physical terms this carry-in is just a wire coming from the previous stage. You see, in practice, numbers are often represented by multiple bits. A single bit would only be able to take on two values (that we conceptually interpret from it), namely yes/no, 1/0 etc. Now with multiple bits, a general rule is that n bits translate into 2^n values. A two-bit number will therefore be able to take on 4 values: 0 (00), 1 (01), 2 (10), 3 (11). 

This assignment of numbers comes from the fact that computes use a base-2 number system (us humans conceptualize this, not the hardware), where each position has a place value which is a power of 2: right bit has place value 2^0 = 1 and left bit 2^1 = 2. So with 01, we simply add the value where the bit is 1, here it’s the right place, so we have value 1. For 11, we have both, so must add 2+1. 

This base refers to the number of symbols we’re allowed to use in each position and as such then we must ‘carry’ to the next one. We have binary numbers here, so after 1 there’s no new symbol, and we must carry to the next place. There exist many bases, such as base-8, base-10 and so on, but computers prefer the base-2 as it forms the natural situation resulting from electricity. 

Now going back to the multi-bit situation to better comprehend this full adder logic, the two-bit scenario with 4 values will have 2 stages. And if we extend the logic of always having A and B inputs, we see the full picture of this two-bit number as:

	A0; A1; 
	B0; B1;

So in stage 0 (A0,B0), we always just have the half adder. There’s two inputs and from their combination we may have no values (00), XOR values (10 or 01) or and values (11). Now in hardware arithmetic logic, this translates in having either nothing happen (0), a sum happen (simply 1) or a carry in the case of the AND gate being activated. This carry must go to the next stage, where it comes in as a carry-in. 

Now at the last stage, you might wonder, what happens if we must carry again? Well there is no next stage to carry to, so the adder block usually just has a wire coming out which is seen as a separate output pin, the overflow. It’s then up to the system designer to make use of this in some way. So after the last stage we’ll see three wires coming out, two for sum, one for carry. 

Realistically there are three options at that point, either treat it as a larger number (100 binary), ignore the third wire as overflow, or connect it to another stage, which is how we can make a 3-bit adder. 

And this is how adding works using the hardware we’ve seen. Using the same tools, all kinds of other operations can be handled, such as subtraction, comparison, absolute values, multiplication, division and so on. They’re not super straightforward and I’m interested to explore, but I’d digress too much. 

**Sequential logic**

Now combinatorial logic has no memory, which means that only current inputs have an impact and that once inputs stop changing, the circuit stops doing anything. But what now if we want to count how many times a button is pressed or have steps going in the right order? That’s the reason we want a circuit whose output depends on the past and not exclusively on the present. 

Such a sequential circuit then has an internal state, meaning something created in the past influences the present. It persists even when the input doesn’t change. The simplest possible question is how we can store just one bit? Well apparently CMOS is quite elegant for this in that if we feed output of a CMOS gate into its own input, the circuit remembers. 

Remember the NOT gate, which had VDD - PMOS - output - NMOS - GND, where PMOS conducts when input = 0. As such we had high output when input was low, and no DC path ever from VDD to GND. Now if we take a wire and connect the output of this inverter gate to its input, then there’s no external signal driving the state of this gate anymore. It’s quite intuitive that a constant fight will start between PMOS and NMOS. Output is low so input becomes low, turning PMOS on and in turn turning output high (and input high), in turn switching NMOS to conduct to ground and turning output low, and so on. There’s a constant back and forth, it oscillates. This doesn’t yet create memory but it does portray the idea of **feedback**, which turns a static logic element into a dynamic one (creating dependence on time and prior values).

Now to get memory we want a state that reinforces instead of destroys itself. So we simply add a second NOT gate after. We now have inverter A and inverter B, where the output of inverter A becomes the input to inverter B. The output of inverter B then again goes to the input of inverter A. This makes two stable equilibrium points of the circuit, either it’s in high or in low. Such a circuit can store information as node voltages, but once power is gone, all memory is gone. Notice that we’re not able to influence it yet, once it settles it won’t ever change. 

The next step is therefore to get external signals to temporarily overpower the feedback, creating a **latch**. It must only do so temporarily, as we sometimes want to get feedback to dominate (hold) and sometimes want input to dominate (write). The simplest way to do this is via the S-R latch. We basically introduced two new elements, S (set) and R (reset). Of our two inverter gates, we also choose one whose output node is now called Q. The output node of the other gate then becomes Q̅ (‘not Q’). 

Now an important conceptual step is the realization that NMOS can pull the output down (to GND) and PMOS can pull it up to VDD. In order to then implement such a latch we basically want to add an extra transistor than can pull an output node down/up when we want. So if we’re at output Q we could add one additional NMOS transistor whose input gate is driven by an external signal. When this NMOS, whose input is determined elsewhere, turns on, Q is forced to low output. Meanwhile when this NMOS is turned off the inverter pair regains control. This external control is the reset (R). 

On the other inverter’s output node, Q̅, we add a symmetric transistor. So we do the exact same thing and add an NMOS transistor there as well (Pull-down only latch). This one is the set (S). Set is called so because it turns on the NMOS that pulls Q̅ down to ground voltage (0), such that in turn Q becomes high (remember they’re interconnected). Reset on the other hand turns Q itself low. 

Visually, such an inverter get looks as follows:

<div style="text-align: center;">
  <img src="/assets/SenseoViz/14/FeedbackInv.png" width="340">
</div>

Because the output is just the metal wire between the two transistors, where we could attach a multimeter and measure the voltage, we can also just attach another transistor to it as well. This other NMOS transistor can then easily be attached to an outside input, and bring the inverter’s output to ground when we choose. Now if PMOS and R/S are on, there is briefly a short circuit, but CMOS inherently has it so that the feedback turns the PMOS off. 

SR latches are often built in a slightly different manner, however, since we might want to actively  pull-up the outcome’s writing instead of pulling down always. We still have two cross-couples inverters A and B which already have memory and store bits. For a canonical SR latch people then usually cross couple the NOR gate we’ve seen before, as these inherently contain pull-up and pull down without additional forces. However, while still interesting, we’ve got the gist of the SR latch idea. 
