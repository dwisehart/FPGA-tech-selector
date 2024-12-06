# FPGA-tech-selector
LTSpice files for simulating a 1 of 8 selector using MOSFET transistors in CMOS pairs.


These are the files for the article published on LinkedIn.  To get started, download and install
the free LTSpice tool from Analog Devices.  Start that program and open SEL-STIM.asc.  Hit the
'run' icon in the upper left of the window.  Enjoy!


Below here is the text from a draft of that article.  Also see the PNG files for the images from
that article.


FPGA tech: Building a selector

Someone was asking me how a selector is built inside an FPGA.  Looking back, I think either I did not describe it very well or I did not answer the question he was asking, so I decided to put a more complete reply here.

A selector is just an electronic way of muxing several signals together into a single output.  Back when electronics were built with discrete ICs--that is, without an FPGA--we could buy an IC that would give us this functionality.  Today, we write some HDL that looks a bit like C-language code and the FPGA tools synthesize our source into an electronic circuit that does what we want.  The question is, what does the finished circuit look like for a selector implemented in an FPGA?

For an 8-input selector, you need three bits of address, which are ports SEL0, SEL1 and SEL2 in the schematics.  These schematics were all created in LTSpice, which is a brilliant and free piece of software allows you to construct and simulate electronic circuits of all types.  If the right bit-address is on those pins--which is the address 111 for the first schematic--the input port shows up on the output port, but inverted (a high input gives a low output and vice versa), making this a NAND circuit built from NMOS and PMOS transistors.  When anything other than 111 shows up as the address, the output sits idle with a high output.

So this circuit takes care of one input, which has the address 111.  There is another one of these circuits for each address.  The circuits only vary by whether there is an inverter on SEL input ports--there are three inverters for address 111--or not--there are no inverters for address 000.  Every other address is somewhere in between with some but not all inputs inverted.

One of the nice things about LTSpice is that it allows us to take a schematic like this and package it into a sub-circuit, and then pull multiple sub-circuits together into a larger circuit as you see below.

On the left side you see a simulation stimulator, which is going to generate different frequencies on each selector input so we can tell them apart in the simulation output.

The next column is the single-bit selectors we have been discussing, with the address written in the lower right corner of each sub-circuit.

The last subcircuit takes the output of whichever selector bit is addressed, and pushes it to the output port.  This subcircuit is identical to the selector bit circuits, but it has 8 inputs instead of 4.

Now with the various sub-circuits created and instantiated, we are ready to simulate.  Push the little running man icon at the upper left, and less than 10 seconds later--for this very simple circuit--the simulation is complete.  Go back and add probes to the parts of the circuitry you are interested in and their waveform appears.

After verifying that the output was what I expected, I probed each of the outputs of the single-bit selectors where they lead to the 8-input NAND.

The stimulator generates eight different frequency but continuous square waves on its OUT ports.  The stimulator counts from 111 down to 000 on the SEL ports, which activates one single bit selector at the time.

OK, now I know it is hard to see the next great trade in an image like that, but believe me, this is great progress.  In part 1 of this article we have created and simulated the functionality of a discrete IC for 1 of 8 selection.  Now that we understand what it is we are trying to build, we can modify the schematics a bit and simulate how an FPGA might approach the same problem.

