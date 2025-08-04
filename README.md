# Goose Belt Purger (GBP)
A simple, lightweight, and low-cost belt purger for 3D printers.\
An alternative to blob-producing purging routines and devices, or traditional purging into towers.\
Originally designed for the Voron Trident, but small and light enough to be adapted for other printers.

  
![](/Assets/Purgers_small.jpg)  
  
See also [YouTube video](https://youtu.be/MWTFuGqhi_c) of purger in action.
  
Currently in Beta testing — proven as a functional design, but can have unexpected issues and design might change any time.  
  
If you have any questions, you can visit my [Discord server](https://discord.gg/zYc39Neu).  
  
You can also find GBP on [Printables](https://www.printables.com/model/1371713-goose-belt-purger-gbp)

# About
Like many makers, I've been fascinated by single-nozzle multicolor/multimaterial printing. But like many, I hate purge towers. They can be large, ugly, and take up valuable print bed space. This is why I was intrigued by the Blobifier — but there are several issues. First, I use a Voron Trident, which isn't compatible. While there are adaptations of the Blobifier concept for the Trident, none have gained significant traction. Second, both the original Blobifier and its Trident adaptations generally share the drawback that each purge requires toolhead parking near the bed edge, blocking part of the bed and potentially risking collisions. Third, the produced blobs are volumetrically inefficient. Even though the blobs are relatively compact, their shape means they can't utilize the waste bin space effectively. This is even worse with the loose coil "poops" produced by some printers.

There had to be a better way.

And then it struck me — use a belt to produce compact "line" purges. To be fair, this isn't an entirely new idea. There have been mods involving purging on stationary aluminum platforms or kinematic belts, but none of those options felt right. I wanted something simple, affordable, and buildable from stuff you already have lying around your workshop.\
This is why the Goose Belt Purger was created.

What does it have to do with geese? Nothing. I just like geese.


# Design

The core of the GBP is a silicone wristband — the kind you can get anywhere, often for free. The most common size, which GBP is designed around, is 12 mm wide and 20 cm in circumference. You can use any similar band, even ones with embossing or printing, since we'll use the smooth inner surface. Changing the belt is easy, so use whatever you have and swap it later if needed.

Silicone rubber is ideal for this use. When filament is extruded onto the smooth side, it sticks but releases easily at the end of the straight belt section. Silicone is heat-resistant enough to endure the nozzle heat, provided it keeps moving. However, if the nozzle stays in one place too long, the band will discolor (brown or black) or even burn.

The belt moves along two printed pulleys. How does it stay in place without flanges? Dark magic. Don't ask.

One pulley is driven by a cheap miniature DC motor with a gearbox.  A 50rpm or slower version is recommended, up to 15rpm if you want really thick extrusions. &#x20;

> [!WARNING]\
> Motor is an inductive component. Skipping the recommended protection (e.g. flyback diode) can cause voltage spikes and permanent damage to your controller.  
>  
>Read the “Electrical connection” section and install all safety components.  
>Use at your own risk.  

The other pulley spins freely on a tilting arm. The arm pivots on a bearing around the motor shaft and is pushed upward by a pair of magnets acting as springs. The arm's purpose is to press against the nozzle but move away to avoid collisions with the nozzle or printhead.

GBP is mounted using an Armored Turtle brush mount or universal Klicky style mount. But you can also design your own mount and connect it with AT mount styled dovetails.

# Bill of Materials (BOM)
## Purger
- 1x Silicone wristband (12 mm wide, 200 mm circumference)
- 1x DC motor, 12 mm diameter, 6V/12V, 15rpm — commonly sold as GA12 N20 or N30. See note below.
- 3x 623 bearings
- 2x 6x3 mm magnets (Voron standard size)
- 4x heat-set insert M3x5x4 (Voron standard size)

**M3 hardware:**

- 1x M3 nut
- 1x M3x6 set screw
- 1x M3x16 SHCS
- 2x M3x20 FHCS
- 1x M3x30 SHCS

**Electrical components:**
- Schottky diode 1A (e.g. 1N5819) - For 5V or 12V supply voltage  
or
- Schottky diode 2A (e.g. 1N5820) - For 24V supply voltage together with 12V motor  
- Resistor 47R
- Ceramic capacitor 10 nF, 50V 

If your board supports 5V on fan outputs, you may use a 6V motor. If only 24V is available, use a 12V motor with <50% PWM.  
Initial testing shows optimal belt speed under load is 2–12rpm, so the 15rpm seems to be ok. PWM can reduce speed to \~5–10% of rated speed. 30rpm and 50rpm motors also seem to be usable  

## Mounts
**AT mount**  
Applies to Voron Trident or similar printer together with modified (shorter) AT mount files  
- 2x heat-set insert M3x5x4 (Voron standard size)
- 2x M3 nyloc nut
- 2x M3x8 SHCS
- 1x M3x35 SHCS (you can use M3x30 if you have trouble getting it)
- 1x M3x25 SHCS

**Klicky style mount**  
Universal solution  
- 8x heat-set insert M3x5x4 (Voron standard size)
- 8x M3x8 SHCS


# Build Instructions

Build instructions can be found [here](assembly.md).

# Electrical Connection

Connect the DC motor to any free power output on your controller board, preferably a fan output. If possible, supply it with 5V or 12V to minimize heating. Otherwise, reduce voltage via PWM.

> [!IMPORTANT]\
> The DC motor is a strong inductive load and will generate large voltage spikes, which can destroy your board's MOSFET or even the MCU if untreated. At minimum, use a properly sized Schottky flyback diode across the board output. I also recommend an RC snubber on the motor terminals.
>
> Some boards (e.g., BTT Octopus) already include protection diodes. Others rely only on the MOSFET's internal diode, which is usually insufficient. You can check comprehensive table [here](board_list.md), or consult your board's schematic.  When unsure, ask someone.
>
> For the RC snubber, I used a 47 Ω resistor and a 10 nF, 50V ceramic capacitor. Solder one leg of each component together, then solder the free legs to the motor terminals. This configuration dampens voltage spikes while preserving PWM response. You may use a larger capacitor for extra safety, but note that increased capacitance degrades PWM response. Avoid using a capacitor alone (without resistor), as this can cause resonance and make things worse.

I confirmed the effectiveness of these measures with an oscilloscope, but I can't guarantee that no fast transients were missed (e.g., during PWM changes). Data is still limited, so proceed with care and understand what you're doing.

# Software Configuration

A configuration file with a macro is available. See comments in the file and adapt it to your printer. Future refinements are expected.

If you're using HappyHare or a similar tool, configure it so it doesn't return to the previous or next G-code position.

Once set up, the purge routine can be triggered by running the G-code macro:

- `goose_purge VOLUME=###` or
- `goose_purge LENGTH=###`\
  where `###` is the purge volume in mm³ or length in mm. If both are provided, `LENGTH` takes precedence. If neither is provided, the macro runs without extruding.

# Slicer Configuration

Place the `goose_purge` macro at the beginning of your start G-code where you'd normally place a prime line. For tool changes, insert it just after the `T# ;` command.

Note that not all slicers support passing purge parameters. In OrcaSlicer, you can use `goose_purge LENGTH=[flush_length]`. In PrusaSlicer, the volume must be hardcoded.

# Tips and Observations
 
Extrude onto the smooth inner side of the wristband. You can try the outer side, but adhesion is generally worse and requires an unembossed, unprinted band — which is surprisingly hard to find. The inner surface sticks well, even without direct nozzle contact, while the outer side requires pressure that magnets alone can't provide.

Adhesion heavily depends on belt speed. If filament doesn't stick, slow the belt. Once adhesion starts, the extruded line tends to pull more material from the nozzle. A part cooling fan can help, though this isn't yet included in the macro.

The macro splits the purge into short "toothpicks" or pellets to improve waste volume efficiency.

Initial testing shows optimal belt speed is 2–12rpm (roughly 1.8–11 mm/s or 108–660 mm/min). The easiest way to measure speed is to mark a spot on a pulley, start a stopwatch, and time 10 full revolutions.

# Risks

This is an early-stage mod and hasn't been extensively tested. Known and unknown risks exist. If issues arise, please report them.

**Known risks:**

- Limited motor lifespan due to mechanical wear
- Motor overheating
- Belt thermal degradation
- Frame deformation due to heat or tension
- Poor purge adhesion or debris returning to the nozzle
- Print failures or damage from stray purged filament


