---
layout: post
title: Cosmic distance, and how they are measured
subtitle: A quick walk through of two cosmic distance measurements
tags: [pop science, astronomy]
comments: true
cover-img: /assets/img/andromeda.jpg
thumbnail-img: /assets/img/andromeda.jpg
share-img: /assets/img/andromeda.jpg
---
Some fun facts: the Sun is one astronomical unit away, which is $$1.5\times 10^8$$ kilometers away from us -- that is seven zeros after 15 -- or 8 minutes 19 seconds away if you travel with the speed of light; the center of the Milky Way is about 8 kpc (kilo-parsec) or about 26000 light years away from us; some of the old supernova extends beyond one billion parsec away; cosmic microwave background (CMB) radiation is about 14 billion parsecs away. Have you ever thought about how distances are measured at the cosmological scale? While that is definitely interesting enough to go through carefully, let's look at two types of distance measurements this time: the method based on how "luminous" an object looks, and that based on how "big" an object looks. Technically, the two methods lead to two different physical quantities, the luminosity distance and the angular distance, but we can skip that technicality for now. 

### Luminosity distance
Now imagine you have an emergency lantern, and you have had it for many years so you know exactly how bright it is. One time you and your friend go caving -- its a very big cave that your friend finds a bit intimidating. Your friend, unfortunately like me, has a very poor direction and tend to get lost very easily. Concerned about your friend, you lend them the emergency lantern and tell them to flash it if they get separated and lost. The moment your friend turns it on, you immediately get enough information to locate them: their direction -- the coordinate projected to the two dimensional sphere around you, and their distance from you. Your friend asks how you could figure that out the distance so easily, you laughed: oh from the lantern I knew you drifted quite far dummy. In retrospect, what you really meant was that "I know my lantern, and based on the brightness I saw from my location, I computed your distance and you turned out to be quite far away from me." Here is the thing: once you know how bright an object is, you can use the "apparent luminosity" to deduce its distance. This is a very important way to measure distances across the universe. Among them, a class of objects called type Ia supernovae play the very important role, (besides earning Adam Riess, Saul Perlmutter, and Brian Schmidt the Nobel Prize in 2011 for providing evidence of the expanding universe is accelerating.)

 Type Ia supernova share the similar luminosity within statistical fluctuations, after performing some quality cut and selection criteria to throw out those atypical or badly measured. For this reason, they are referred to the standard candles. I'm sure now you can guess the idea of using type Ia supernovae to measure the distance: we first learn how bright they are by studying those close by. Given that they are close to us, we can learn their distance with other method, such as parallax. We thus know how intrinsically bright all type Ia supernovae are. This is the step that is commonly referred to as _anchoring the supernova_. Now we look at those far away, based on how bright they _look like_, we can deduce their distance. Since the _intrinsic luminosity_ of the type Ia supernovae is still an input, one that relies on other distance measurement method, and the deduced distance from type Ia supernovae can be used to gauge other methods, we call this regime the _distance ladder_. The distance measured this way is the so-called luminosity distance. Interestingly, this method also finds its application in Wi-Fi positioning system.[^1]

### Angular diameter distance
Next time you go camping with your friend, and they learned their lesson: they brought their own lantern and showed it to you. Soon after it gets dark, without much surprise, they get lost again. You see the lantern is turned on, yet you also realized that it's not your lantern. That means you don't know their brightness beforehand. Nevertheless, you again quickly get their location coordinate by recalling how big the lantern was. By staring at the tiny light spot, you know that you'd better hurry before your friend completely disappear following their random walk. This shows another method of measuring distance -- by looking at an extended object whose size is known. By measuring the angle it subtends, we can estimate the distance of the object. 

There're good examples of applying this method in cosmology. One of them is the well known baryonic acoustic oscillation (BAO). After the universe was born as a hot and dense soup of radiation, it keeps cooling down due to its expansion. When it's about 379,000 years old, the scattering between photons and baryons became rare enough such that photons start to free-stream. This process is referred to as photon decoupling. Before this point, the photon-baryon system had an density that was oscillating due to the pressure. The pressure wave propagates with a speed that's roughly constant, in the same way as sound propagates in the air. After the decoupling, the size of this oscillation, the sound horizon, is imprinted in the baryons left behind by the escaping photons. With a few inputs, the sound horizon can be computed to good precision. This is our extended object, the lantern your friend brought and showed you. By measuring the CMB, the angular size of the BAO is observed. From this, we can determine how far the CMB is.[^2]

This way of measuring cosmic distances itself is really cool. What's even cooler is that it can be used to probe/ constrain new physics. I will elaborate in a future post on this, which also involves some of my own work.  


[^1]: Wiki tells me that received signal strength indication (RSSI) plus trilateration algorithm is one of the four common classes of Wi-Fi based positioning system, with the other three being fingerprinting, angle of arrival (AoA), and time of flight (ToF).

[^2]: The actual determination of CMB distance is a bit more complicated that involves a global fit of six cosmological parameters, with more observable than just the BAO peak. In particular, another important angular scale, the Silk damping also puts a constraint in the parameter space. 