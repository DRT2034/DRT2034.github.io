---
layout: post
title:  "PCB Microcontroller Subsystems: GPIO - 1"
date:   2026-02-06 06:38:00 +0200
categories: Senseo 
published: true
---

Next in the MCU we encounter the GPIO - General Purpose Input Output pins. We’ve seen the CPU, which is essentially millions of flip flops connected via combinatorial logic and driven by a clock. A bit (binary digit) that the CPU understands is one that is synchronized by the clock, lives in a flip flop and has setup and hold constraints. The CPU is kind of like a self contained box when it comes to bit reading. We cannot simply connect wires coming from all places, especially when it comes to components like a boiler which are driven by AC 230V instead of the electronics DC 3V or less. 

That’s where the GPIO pins come in, their job is to act as a buffer and translator between the noisy outside world and the strict clocked inside world of the CPU. It adds input buffers, synchronization latches, optional filtering, output drivers and protection circuitry. It’s a circuit that makes an external voltage safe to turn into a CPU bit. When we for instance push a button on the machine, a wire in the machine drives this to the CPU, but will first stop at a GPIO pin, where an input buffer converts voltage into a logic level. The signal from this is then synchronized to the clock, the value is stored in the flip flop and the CPU then reads that flip flop. 

Now what is this synchronization? The clock driving the CPU is pretty self contained in that it has VDD driving a quartz which in turn generates a sine wave, which is turned into a square wave (clean 0 1) with an inverter. The CPU has absolutely no idea when the outside world changes. So how do we safely take a voltage that can change at any time (a button press for instance) and turn it into a clocked bit? The idea here is to wait until the clock edge and then sample whatever voltage is available to be sampled. 

**Pin MUX** 

There’s this pin at the edge of the MCU which lets outside systems connect to a certain particular subsystem of the MCU. This pin then feeds just one block and that block decides what the signal means. We therefore have a multiplexer (MUX, the same exact building block as used in the PC of the CPU) that decides to connect the pin either to the GPIO, ADC, timer etc. Note that the GPIO isn’t a filter for the whole MCU, it simply exists to give the CPU (the brain) a generic way to read or drive a pin as a logic value. 

Without the GPIO, the voltage the CPU must read would be quite messy. So when the CPU wants to read or drive signals in the machine, it needs the GPIO. For the Senseo machine this comes down to tasks such as driving the brew button and whether the tank is present. These are anything software isn’t, they’re human driven and completely uncoordinated. Same goes for the LED buttons that blink, which are driven directly by the CPU, through the GPIO.

Now in the MCU we have many other components (called peripherals) that also use the pin, such as the ADC (boiler) and the timer (which gets used for pump speed (PWM) and capturing flow sensing). These all share that same pin and the multiplexer selects which one gets connected to the pin at a certain time. 

The reason we switch between peripherals when plugging to the MCU connection is that if we’d connect multiple at once, we get a lot of noise. If the ADC were connected together with the GPIO, the Schmitt trigger would inject noise and the accuracy of boiler temperature sensing collapses. 

**Input Buffer: Schmitt Trigger** 

So we have an external voltage (e.g. button or sensor) that can change at any time regardless of clock or anything else inside the CPU. This is called an analog voltage. The GPIO then first uses an input buffer (often the Schmitt trigger ~ Dokić type) to clean up that voltage and turn it into logic. This Schmitt trigger comparator turns noisy and imperfect analog voltage into a clean digital 0 or 1. It’s the same Hysteresis concept we’ve seen at the boiler heating. We have 2 threshold, e.g. V-t+ and V-t- where if the noisy voltage exceeds V-t+ (falls below V-t-), the buffer turns it into a clean 1(0). 

Now you might think such a hysteresis band would become messy if we have both boiler of 230V and sensors of 3V using it. The thing is however, there is no GPIO anywhere near the boiler, the boiler’s voltage goes through a bunch of places before it reaches a GPIO, where it’s already turned into 3V DC. The electronics world we’re now operating in is purely DC small voltages. What this Schmitt trigger essentially ensures is that a ‘weak’ push of the button and a strong one have the exact same effect in the MCU, so that both elicit the exact same response. Yet despite the boiler’s 230V being leveled down, it’s still extremely volatile and noisy, so likewise it needs stabilizing before being sensed by the CPU. 

The Schmitt trigger has the main goal of creating hysteresis, meaning that noisy input only leads to clean output by requiring to pass a certain threshold, both upward (PMOS pull-up) and downward (NMOS pull-down). Now there are many designs used in practice, even without sub-models of design. As such it can get a little tricky getting the mental model exactly right. I’ll focus on the original Dokic design from 1983, as going directly to the source provides the most reliable material. Nowadays, however, this Dokic Schmitt trigger is a little different, and usually uses 6 MOSFET transistors and even some other novel components. But let’s sit with the basics: 

We have a main stack of regular CMOS transistors placed in formation like the inverter 

VDD - P1 - Output - N1 - GND 

But to this stack we add some other components, namely two resistors and two extra feedback transistors. Now as discusses before, such resistors are pretty passive devices, whose sole job is to resist the current coming from the rails in this case. By isolating the inverter source pins from the supply rails (VDD/GND), these resistors allow these source voltages to be modified by the output through the feedback transistors. Without such resistors, the PMOS and NMOS source (see S below) would be clammed hard to VDD/GND. By adding these resistors internal nodes are created to which these source pins are attached. Visualizing this gives us: 

<div style="text-align: center;">
  <img src="/assets/SenseoViz/18/SchmittCircuit.png" width="340">
</div>

Note that the gates of these feedback transistors are tied to the output, effectively providing us a sort of memory feedback in the sense that the output given by the previous state determines the impact of current input. Further, the source of these feedback transistors are tied to the other side’s rail (don’t mind the drawing).Now consider the following: 

At a particular moment, we find input (gates of P1/N1) and it is **high**. This in turn means N1 is on, P1 is off. If N1 is on, that means there is a path to ground and the output has been pulled low. So far so good. Now if output is low, that means the gate of P-fb triggers this feedback transistor on. As such there is a conducting path from GND to the internal upward node, pulling this node low. This has no immediate effect, because with input high, P1 doesn’t conduct anyway. But now consider what happens when input is **falling**. This means that N1 will gradually taper off, and P1 will equally gradually turn on. But remember that for P1 to turn on, we had that VSG > Vth (V-source - V-gate > internal switching threshold) and we saw that the upward internal node is being pulled to GND. The direct effect of this is that V-source of P1 is near 0V, and we essentially have 0 - V-gate where gate is the falling input (and P1 turns on if input low). The input must therefore fall lower in order to have VSG exceed the switching threshold. So P1 will turn on and pull the output to VDD, but at a biased moment, when when input is lower than it otherwise would've needed to be. So now we come to input being **low** and output having consequently turned high (P1 conducts VDD to output). With output high, P-fb has turned off and N-fb has turned on, resulting in the downward internal node being pulled high. But again nothing happens, because input is low and N1 doesn’t conduct. Whenever input starts **rising**, however, a similar story takes place. For N1 to turn on, VGS > Vth (V-gate - V-source > internal switching threshold) so that with a source that is pulled high, gate (input) must rise further before VGS is higher than the switching threshold. Whenever that threshold is exceeded, N1 conducts and the output is pulled to GND, switching off N-fb and turning on P-fb. That’s the story of hysteresis. Two thresholds are created through feedback, and noisy switching is preventing. 

**Synchronization**

From the input buffer, we’ve now received a clean 0 or 1 based on the outside analog signals. Before getting this into the MCU, however, we want to synchronize it to the clock edge, so that it plays nicely with the other signals in the CPU core. What’s done in order to achieve this is that the output node of the Schmitt trigger is attached to the input of a flip-flop. And remember from before, the flip flop’s ‘enable’ can be attached to the (same) clock as the other ones in the MCU. So the D flip flop takes in the stable input and synchronizes it to the clock edge. 

The major problem is that the rules of the flip flop will be broken through timing. It actually has two rules, namely setup time (input must be stable before clock edge) and hold time (input must remain stable after clock edge). What happens, though, is that the input transition at times happens near clock edge but not exactly on it. This is unavoidable for a GPIO because buttons being pressed, sensors going off and other external clocks all depend on timing, and have nothing to do with the noise cleaning the Schmitt trigger does. 

So what happens is that we have our flip flop, which is just two cross-coupled inverters and a few transmission gates, and the clock edge (the enable on the transmission gate) happens when the input is changing. At such a moment, the internal nodes of the flip flop will enter a metastable state because one will try to become 0 and the other 1 (detect the input). This metastability means nothing more than that the output will sit at an intermediate voltage longer than we’d like it to. So not 0 or 1, but somewhere in between, which leads to thermal noise or mismatching at a moment we cannot predict when it will happen. Such metastability will break data paths further on as a result of short glitching or different values in different paths. The worst thing is that it's a hardware phenomenon, so no software will even detect this. 

<img src="/assets/SenseoViz/18/ExponentialPDF.png"
    width="200"
    align="right"
    style="margin: 0 0 1em 1em;"> 
The natural solution, however, is to just use a second flip flop and make the synchronization process a synchronized chain. Our first flip flop might go metastable, but as long as it settles before the next clock edge sends the bit into the second flip flop, we’ll get a valid bit out of the second one. The probability of the state not settling before that edge follows an exponential distribution (see probability density function on the right - disregard text). 

Once the bit comes out of the second flip flop, it’ll only change on the clock edge and be fully synchronous to the other parts of the MCU. 
