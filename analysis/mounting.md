# Mounting Hardware

While surface mount standoffs are probably ideal, for a first revision I want to try and avoid things that may be hard to solder, and surface mount standoffs are typically big thermal masses that make manual rework challenging.

Instead, we will use threaded female hex standoffs.
From some rough measurements, there is approximately 8.5mm of clearance above the top surface of the SMC PCB (assuming the SMC PCB is 0.8mm thick).
McMaster-Carr sells a [6mm long standoff](https://www.mcmaster.com/94868A161/), that leaves us with about 2.5mm of headroom.
> Note: The Pi's height with all connectors removed except for the SD card is about 2.9mm, which means it will probably have to go on the bottom of the compute PCB

For screwing the PCBs to the standoffs, we can use these [3mm screws](https://www.mcmaster.com/92095A113/), which have a head height of 1.5mm, which puts us just barely under our remaining headroom when accounting for the PCB's 0.8mm thickness
