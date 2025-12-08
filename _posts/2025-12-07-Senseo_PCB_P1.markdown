---
layout: post
title:  "Printed Circuit Boards: Fundamentals 1"
date:   2025-12-07 06:19:21 +0200
categories: Senseo 
published: true
---

## Bits and pieces 

<img src="/assets/SenseoViz/4/SenseoPCB.jpeg"
    width="250"
    align="right"
    style="margin: 0 0 1em 1em;">
Let’s now start with arguably the most difficult sections, the ones exploring printed circuit (or wiring) boards. In it’s essence, it functions as a fixed path for components to have connections to their counterparts. Instead of having wires connect the components electrically, the PCB does it via copper pathways. So most of the board is insulated, remember from the boiler pipe that this refers to materials not allowing electrons to flow. Certain specifically directed tracks on the board are then made of copper to permit that flow.

### Fire Retardant 4

The insulating material in the case of PCB’s is fiberglass and epoxy, whose combination is also called FR4 - fire retardant [factor] 4. The idea with fiberglass is that [glass]({% post_url 2024-10-02-Glass %}) is made into a kind of woven fabric that leads us to have thin fibers. So it’s literally just glass woven into microscopic threads. The result is a very strong, heat resistant and non-conductive material. The reason this is done is because glass on its own breaks easily, yet has the good qualities we seek for this board. Once molten glass is woven into fibers, however, it becomes extremely strong and flexible while retaining those attractive qualities. 

Epoxy resin is a little more intricate, as its a type of hard plastic. But what is plastic? Well I wrote a [little exploration]({% post_url 2025-11-30-Plastic_Origins %}) on it. Polymer, these very long chains of small repeating molecules linked to each other, likewise have the quality of not conducting electricity while bringing the additional advantage of having a moldable design. In fact, epoxy resin isn’t just a polymer, but a thermosetting polymer, meaning that it's a type of plastic that is first liquid and upon heating (or a chemical reaction) will become very hard and never again lose its shape (from newer heating). The result is a ceramic like hard plastic. 

The combination of the two leads to a reinforced plate that’s well insulated. A good analogy for this is reinforced concrete. Cement is hard but brittle, steel rebars are strong but flexible, yet together they form something strong and rigid. FR4, our combined material, is as such electrically insulating, flame resistant, easily operable and holds copper very well, which makes it perfect for electronics. 

There are some additional layers aside from the FR4. The first is that of copper pathways carrying the electricity from component to component. Then, a sort of colored coating (brown or green) goes on top of the copper and prevents short circuiting. On the very top, then, we can see a silkscreen holding white printed text, labels and outlines of the components.

The big help of this board is the replacement of wiring for very intricate actions, creating a very neat and efficient place for essential electronic actions to take place. We find three different kinds of copper on the board, being power traces, signal traces and ground planes. The power traces have the simple task of carrying electricity from the power supply to a destination. Signal traces carry data or control signals, and ground planes are used to have a reference layer (other side of the board) and reduce electrical noise. 

### Data Through Voltage & Transistors

Personally, I get a bit confounded by the phrase ‘carry data or control signals’ for the signal traces. What does that mean? It kind of distracts me towards another topic I’m eager to explore, which is how people have managed to put music into the physical object of a vinyl record, which then plays when a needle goes through it.

Well PCB signal traces are just wires, which can do only one thing which is let electrical signals travel along them. An electrical signal is where changes in voltage are used and encoded with data. It is the equivalent of signaling with morse code and a flash light, where ON, OFF and length are used to carry information. With the PCB, it’s the exact same idea, but with voltage. High voltage and low voltage are the two options, turning into 1’s and 0’s respectively and becoming bits, the simplest unit of data. 

The way this is done is through the microcontroller (on which more later), which has transistors in it (remember Moore’s Law). Transistors are like tiny switches, turning the flow of electrons on or off, causing voltage to rise and fall on the copper trace. Another chip at the other end of the trace picks this up and interprets the pattern. Data is therefore merely timed voltage changes. As we’ve seen, however, it isn’t really electrons that are flowing through the wire, as they just jiggle in place (think back to the pendulum swing). The changing current is what results in an electromagnetic wave travelling at nearly the speed of light.

Now different chips must understand each other, just like we agree on the morse code alphabet. The way chips do it is through Communication Protocols, which is the shared language (like USB, Ethernet, HDMI etc.). Chips are designed in a specific manner to be able to understand each other. If we have a microcontroller, the counterparty might be a sensor (temperature or light), a memory chip (like RAM), and so on. The microcontroller uses the transistors to switch the output pin to Vcc (high voltage) or ground (low voltage).  

Let’s take a step back and consider transistors at their base:

We’ve already considered voltage in depth, where pressure difference results in current of electrons being pushed around. The wire (copper trace in this instance) forms the path electrons can move on, i.e. wiggle around on, the pressure difference results in a sort of ripple effect that is felt immediately on the other side. 

Now if the pressure difference between two points is changed, such as a tap-tap—-tap——taptap and so on, than this can be turned into information. Which is done with high voltage (1) and low voltage (0) as well. The thing that causes this changing voltage are **transistors**, which function as a sort of microscopic light switch that is extremely fast. It reacts to tiny voltage being applied to its input, turning ON, and if removed turning OFF. In a CPU (laptop) it can switch at 3-5 GHz (one switch takes 0.00000000033 seconds) while for a typical MOSFET transistor it will take 0.0000001 seconds (10 MHz) which is still very fast. It can do this so fast because it’s microscopically small and the force (voltage) is instant. A transistor then has three important terminals, namely gate, drain and source (for MOSFET), and whenever voltage is applied to the transistor’s gate, drain and source get connected. 

But what makes the transistor switch, where does it all come from? Well the truth seems to be that it’s other transistors. Inside the microcontroller, transistors are connected in patterns forming logic circuits, which send signals to each other. What lies at the base of those is a circuit called a **clock**, but let’s start with logic gates.

