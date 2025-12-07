---
layout: post
title:  "Senseo Boiler: Safety"
date:   2025-11-28 6:20:31 +0200
categories: Senseo
published: true
---

## Bimetal thermal switch

<img src="/assets/SenseoViz/7/BoilerTopAnnotated.png"
     width="150"
     align="right"
     style="margin: 0 0 1em 1em;">
If we look on the image with arrows, we see some components that haven’t been identified yet. We’ve already seen the NTC thermistor (entering the side), the two red terminals, earth, live and neutral wires, but three pieces remain unknown (of which only one has any real importance). If we look at number 1 on the image, that is the bimetal thermal switch. It’s a cylindrical piece attached to the top, in between the live wire and its terminal. 

Given its position between live and terminal, we might already be able to guess its function. Namely, it’s a little temperature safety switch. In fact, all the remaining pieces here just have to do with safety. In the case of the bimetal thermal switch, it cuts the power when the boiler gets too hot. This isn’t a nuclear option, however, it functions as a thermostat that automatically cuts power and turns it back on when the boiler cools down. This way, even if the NTC thermistor, microcontroller and so on fail, the boiler has a reflex of jumping in and protecting itself. 

The idea of a bimetallic strip is that there are two strips of different metals that expand at different rates when heated. One of the metals expands a lot when heated, while the other much less, this way, because they are stuck together, they bend when heat is applied. This bending is used like a tiny mechanical spring opening or closing electrical contacts. 

Let’s again start with the physics. It turns out that when metals are heated, they expand, but different metals do so at different rates (expansion coefficients). This is because heat makes atoms (of which things are made up off) jiggle more, in turn pushing each other harder and farther apart. Now if the average distance between atoms gets bigger, the material as a whole stretches a little. The atoms are bound together by a spring-like force (interatomic bond strength), and depending on the stiffness of this ‘force’, a metal will expand more or less That’s the idea behind these expansion coefficients.

<img src="/assets/SenseoViz/7/BimetalStrip.png"
     width="150"
     align="right"
     style="margin: 0 0 1em 1em;">
If we were to mount two metals with different expansion coefficients together, where one will expand more than the other under heat, this strip of two metals will bend. One metal will want to expand a little, while the other does a lot, so if these metals are welded together, they need to stay the same total length along the joint surface. The metal that expands more will pull the whole into a curve and bend the strip. It bends with the expanding metal on the outside, letting it become a little longer, whereas the less-expanding metal goes on the inside as it’s proportionally shorter.

This is what brings us the cylindrical disks. If we make this strip slightly hill-shaped (think Captain America’s shield), then being heated or not leads to a bi-stability state, where depending on the temperature it either pops outward or inward. That’s how this is manipulated in a way to pop under certain temperatures, by making it extra convex (hill shaped if looking at the outside) or less so. If heat is applied, the expanding metal that is on the inside of the strip will want to expand, but since the disc is already bended in the other direction, it results in stress being put in it while staying in the same shape. Whenever there’s enough stress (certain temperature), the disc flips into the other shape like a metal jelly jar clicking or umbrella inverting. Apologies for the bad quality drawing, it’s the best my webcam could provide at this hour. 

<img src="/assets/SenseoViz/7/BimetalStrip2.png"
     width="150"
     align="right"
     style="margin: 0 0 1em 1em;">
With our bimetal thermal switch on the boiler the idea is that on side of the metal is fixed to the terminal, while the other side is connected to the terminal of the wire (or vice versa) with a spring. This way, if temperature increases, the contact of that one side comes loose from the wire, and the circuit is opened. No electricity can then flow through and the boiler heating is stopped. Whenever the temperature is sufficiently cooling down, the metal goes back to normal and the circuit is closed (connected) again. This snap occurs fast, so there is a reliably fast ON/OFF happening. What engineers do is pre-bend the metal and choose the correct thickness and curvature in order to tune the temperature at which the disk snaps.

In the Senseo boiler, this mechanism is really intuitive. We see in the picture at the beginning of the section that the switch is in between the terminal and the live wire. If the temperature reaches some 110 °C (reliably above normal brewing temperature), the the contact opens and the power is cut. 

## Thermal fuse (cutoff)

We also have the Thermal fuse, which is the safety of last resort. Once this goes off, the boiler is broken and needs to be replaced. Unfortunately, I couldn’t actually find this one on mine. The idea that there’s a metal cylinder with a temperature sensitive pellet inside, and when a specific temperature is reached (around 160 °C) the pellet melts and it’s spring pulls apart the contact. 

It turns out, however, that some later models, such as this one from 2019 likely is, have the thermal fuse also in the black cylinder, which wires the cutoff pellet internally. Although I’m more then tempted to take apart this black chamber, I fear I won’t be able to put it back together anymore as a result. 