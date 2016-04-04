---
layout: post
title: Developing Theia's EOG Signal Acquisition API
---

So you need some preliminary information to understand the problem statement I understand. For the past 8 months I have been working on Theia, my Computer Science Capstone course. My team is developing a EOG (Electrooculography) eye tracker with object identification. What this means is that electrodes are attached to the area around the eyes, and we use them to track the general direction in which you are looking. This information gets passed through all sorts of libraries of math, signal aquisition, machine learning, and signal filtering and pops out as a coordinate in a defined space. At this point, that coordinate is correlated with a video stream from an augmented reality headset. With the image and coordinate information coalesced, we can further define the area and objects you are focusing on. This is the current problem space of the problem. I have been working on the image processing, EOG signal acquisition, and general module integrations with ICs. The following is my exactly copied engineering update on our projects webpage. Enjoy.

The ability for a computer to communicate with our Signal Acquisition Amplifier ([gMOBIlab](http://www.gtec.at/Products/Hardware-and-Accessories/g.MOBIlab-Specs-Features)) is of utmost importance. Without a cohesive communication system between these two systems, there would be no EOG data to process. Who woulda thunk it? Beginning in early October, I started working with the Speech and Applied Neuroscience (SAN) lab under the guidence of Dr. Jon Brumburg developing a robust API in Python and it's C integrated language Cython to communicate with the gMOBIlab. I developed the API to be as general as possible so that no matter the experiment, it would be easily scripted and configured.</p>
<p>Starting in October, I began augmenting, rewriting, and testing Cython code that the SAN lab had developed for a Python integratable API. While the code was operational, it did not configure the amplifier correctly nor handle control flow and errors properly. Because of this, I began working with the [ConfigParser](https://docs.python.org/2/library/configparser.html) Python library that provides a basic configuration parsing language based off of Microsoft's INI file structure.
```
[channel1]
highpass = 0.5
lowpass = 30.0
sensitivity = 500.0
samplerate = 256.0
```
This is a code sample that defines one of the EOG electrode channels. Each channel has built in variables in the gMOBIlab hardware that can be specified. Here I am defining the highpass and lowpass filters to their default values and setting the max sensitivite and samplerate.

In Theia the API is configured in ```channels.cfg```, and the API is implemented in <code>mobilab.py</code> which is the module in which the acquisition occurs and the Kalman filter is applied.

##Ventures with a Raspberry Pi 3 and the NeuralTalk2 Repository

Over the past two weeks, I have been putting the finishing touches on the gMOBIlab Python API (a job that never seems complete) and began researching a computer vision library that I had come across a few months ago as well as try to implement our code base on a Raspberry Pi 3.

[NeuralTalk2](https://github.com/karpathy/neuraltalk2) is an image captioning Recurrent Neural Network written by two Standford graduate students [\@karpathy](https://github.com/karpathy) and [\@jcjohnson](https://github.com/jcjohnson). I have the library set up to run on a virtual machine for testing as well as my MacBook Pro, and the results are promising. Furthermore, we are working on getting a Amazon AWS GPU instance so that we can train a better model more specified to our needs.

Here is a video demonstrating the abilities of this library on a leisurely walk through Amsterdam.

   <iframe src="https://player.vimeo.com/video/146492001" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
   <p><a href="https://vimeo.com/146492001">NeuralTalk and Walk</a> from <a href="https://vimeo.com/kylemcdonald">Kyle McDonald</a> on <a href="https://vimeo.com">Vimeo</a>.

In addition to possibly integrating NeuralTalk2, I am working on implementing our entire codebase on a new Raspberry Pi 3. However, I ran into a large, possibly impassable, barrier. The gMOBIlab Linux API binary file is built for a 64 bit OS. While it is true that the RPi 3 is 64 bit ARMv8 architecture, no OS developers have a 64 bit OS for the RPi 3. The reasoning seems to be that they do not want to support two separate OS builds (32 and 64 bit), and that the RPi 3 only has 1GB of RAM. It appears that the consensus is that 1GB of memory is not enough to fully take advantage of 64 bit instructions. Because of this, I am in contact with gtec and Dr. Brumberg on a solution. We may be looking to purchase a different IC than the RPi 3.
