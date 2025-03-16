---
layout: post
title:  "I am an astronomer!"
date:   2025-03-16 09:00:00 +0100
categories: astro
---

There's a curse we use in Poland - _May you live in interesting times_. Just one of the reasons why it took me so long with this post...

Luckily, this was not the only reason. Death Stranding (damn, this is so good!!!), Tomb Rider(s), Cyberpunk and couple other “played” a role too. Not to mention catching up with the first Dune **trilogy**. I haven't seen it coming - my shortest summary for the books.

And then something  magical happened. I got a telescope from Santa!

An all-time classic - Skywatcher Heritage 130P. A good old fashion Dobson. Just perfect for an amateur I am. 

Aiming it as complicated as with medieval cannon – just grab the tube and point somewhere. Luckily, there’s a red-dot finder, so by far better precision. Greatly appreciated when trying 65x or 130x magnification.

And then there’s the sensation of seeing Lunar craters so crisp and “close” for the first time. Jupiter with its clouds (if conditions are right and you look closely) and Moons. Saturn and its rings! Looking at a star – to find there’s so many more, just too dim to be seen with a naked eye! I could spend hours talking about things like this!

Anyway, this post was supposed to show my attempts in visualizing Sun-Earth Lagrange points (by simulating acceleration field in Solar System). You may have realized by now – not this time.

Obsessed with the telescope, I soon focused on planets. And then the inevitable question – what would I be able to see today?

Of course there are apps like Stellarium (which I use almost compulsively now), but that was not what I was after. What if I took eagle-eye view at the Solar system? Can I build a mental map of what-is-where today?

Pretty soon I ended up with this (source code [here](https://github.com/itsthere-academy/code-samples/blob/main/src/solar_bodies/solar_bodies.ipynb)):

{% include_relative includes/2025-03-16-all-planets.html %}

Not the prettiest thing I will admit. And pretty much unreadable :) Yet, made me see what do they mean by Inner and Outer Solar System!

So let’s plot the inner Solar System:

{% include_relative includes/2025-03-16-inner-solar.html %}

Now the Outer (with Earth added):

{% include_relative includes/2025-03-16-outer-with-earth.html %}

You see this? Now it’s so clear to understand why you can only see Venus and Mercury after dusk, then before dawn. Unless you know how to do it with the Sun high in the sky!

Then I noticed Jupiter should be just up the in the night sky, proud and bright - and it was. And I should hurry with Saturn before obscured by Sun for long months - and I made it. And yes – jaw dropped seeing the rings.

I have a new habit now. When the sky is clear, I go out to see how everything changes. How the skies “drift” to the west. Just to keep an eye on the universe if it behaves the way it should.

You’re welcome :)

P.S. Moon passing by - short recording I took with my phone sticked to the eye piece of the telescope. This is real time (no speed ups). I promise you – looks even better in reality! Yeah, you can find better quality shots online, but I’m happier with mine.

<video style="width: 80%; max-width: 800px;" controls>
  <source src="https://s3.eu-central-1.amazonaws.com/files.itsthere.academy/Moon-passing-by.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
