# Explanation

## Twisted Pair
- A twisted pair consists of two insulated copper wires, typically about 1 mm thick.
- The wires are twisted together in a helical form, just like a DNA molecule.
- When the wires are twisted, the waves from different twists cancel out, so the wire radiates less effectively.
- A signal is usually carried as the difference in voltage between two wires.
	- This provides immunity to external noise because the noise tends to affect both wires the same, leaving the difference unchanged.
- The most common application of the twisted pair is the telephone system.
- The garden variety deployed in many office buildings is called Category 5 cabling, or "Cat 5".
- Different LAN standards may use the twisted pairs differently.
	- Example: 100-Mbps Ethernet uses two (out of the four) pairs, one pair for each direction.
- Illustration: ![[Category 5 UTP with four twisted pairs.png]]

## Coaxial Cable
- A coaxial cable consists of a stiff copper wire as the core, surrounded by an insulating material.
- The insulator is encased by a cylindrical conductor, often as a closely woven braided mesh.
- The outer conductor is covered in a protective plastic sheath.
- The construction and shielding of the coaxial cable give it a good combination of high bandwidth and excellent noise immunity.
- It has better shielding and greater bandwidth than unshielded twisted pairs, so it can span longer distances at higher speeds.
- Modern cables have a bandwidth of up to a few GHz.
- Kinds:
	- **50-ohm cable**: commonly used when it is intended for digital transmission from the start.
	- **75-ohm cable**: is commonly used for analog transmission and cable television.
- Illustration: ![[Coaxial cable.png]]

## Fiber Optics
- Fiber optics are used for long-haul transmission in network backbones, high-speed LANs, and high-speed Internet access such as FttH (Fiber to the Home).

### Mechanism
- An optical transmission system has three key components: the light source, the transmission medium, and the detector.
-  Conventionally, a pulse of light indicates a 1 bit and the absence of light indicates a 0 bit.
- The transmission medium is an ultra-thin fiber of glass.
- The detector generates an electrical pulse when light falls on it.
- By attaching a light source to one end of an optical fiber and a detector to the other, we have a unidirectional data transmission system that accepts an electrical signal, converts and transmits it by light pulses, and then reconverts the output to an electrical signal at the receiving end.
- Two kinds of light sources are typically used to do the signaling:
	- LEDs (Light Emitting Diodes)
	- Semiconductor lasers.
- The receiving end of an optical fiber consists of a photodiode, which gives off an electrical pulse when struck by light.
	- The response time of photodiodes, which convert the signal from the optical to the electrical domain, limits data rates to about 100 Gbps.

### Fiber Cables
- The core of a fiber cable is surrounded by a glass cladding with a lower index of refraction that the core, to keep all the light in the core.
- A thin plastic jacket protects the cladding.
- Fibers are typically grouped in bundles, protected by an outer sheath.
- Terrestrial fiber sheaths are normally laid in the ground within a meter of the surface.
- Illustration: ![[Fiber cable.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 2.2