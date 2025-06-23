# Goose Belt Purger (GBP)
Simple, light and cheap belt purger for 3D printers  
An alternative to blob producing purging routines and devices or traditional purging into purge towers.  
Designed for Voron Trident, but is small and light enough to be adapted for other printers.    
  
![](/Assets/IMG20250603203242_crop.jpg)
  
Currently in early Alpha state - proven as functional concept and can be integrated into printer, but lacks extensive testing to prove long term viability and effectivity. Major refining and redesigns of the design are expected.  
Considering the above stated project status **DO NOT SHARE** this project or any information about it without prior consent  

# About
Like so many makers, I have been fascinated by single nozzle multicolor/multimaterial printing. But like so many I hate the purging towers. They can be pretty huge, ugly and occupy prized room on a print bed. For this reason I have been fascinated by the existence of Blobifier - but there are several issues. First is that I have a Voron Trident, which is not compatible. Although there are modifications of Blobifier idea for Trident, none of them seem to have wider user base. Second, both original Blobifier and Trident modifications generaly share unpleasant trait, that every purge requires a toolhead parking near a bed edge, blocking part of the bed and possibly having collision issues with printed object. Third issue is volume inefficiency of produced blobs. Although the blobs themselves are pretty compact, blob shape means they can never utilize blob bin efficiently. This is even worse with loose coil "poops" produced by some printers.   
  
There had to be a better way  
  
And then it struck me - use a belt to produce compact "line" purges. To be fair, this is not a completely new idea, there have already been mods with purging on a stationary aluminium platforms or on kinematic belt, but none of them seemed right for me. I wanted something simple, something afordable, something you can build with stuff that is already laying around your workshop.  
This is why Goose Belt Purger was created  
  
What does it have to do with goose? Nothing, I just like geese.

# Design
Core of the GBP, the belt, is omnipresent silicone bracelet. Kind of you can get anywhere, often for free. The most common size, for which GBP is designed, is 12 mm width and 20 cm circumference. You can use any that you can find, including embossed or printed on pieces, because we are going to use smooth inner surface. And changing of the belt is easy, so use whatever you have on your hand and you can swap it later.  
Silicone rubber is just the right material for this use. When extruded on a smooth side, molten filament sticks to it but separates easily at the end of a straight section of belt. Silicone itself is fairly heat resistant and as long it keeps moving, should withstand heat indefinitely. But keep in mind, it WILL burn (or more precisely turn to brown and black) if you keep nozzle too long on a same spot.  
  
Belt is moving along two printed rollers. How does it hold on those rollers without any kind of flanges? Dark magic, don't ask.

One of the rollers is driven by a cheap miniature DC motor with transmision. I used a 12V 300rpm version which is powerful enough to drive the belt, however 300rpm is too fast for practical use, so you should get version with 50rpm or less. Although motor is rated for 12V, you can also most likely power it with 24V assuming you drive it with less than 50% PWM - as long you do not overheat the motor, voltage should not matter. Note: In my case motor driven with steady 12V heats itself to around 60°C in ambient temperature, which means overheating in hot chamber is certainly possible. Make sure to use low enough PWM duty cycle to keep the temperature reasonable.  
> [!WARNING]
> Make sure to read and understand comments in section "Electrical connection". Risk of permanent damage to your controller board!
  
Other roller is free spinning on a tilting arm. Arm can tilt on a bearing around motor shaft and is pushed upward by a pair of magnets acting as a spring. Arms' purpose is to push against the nozzle, but move away to prevent a collision with the nozzle or any part of the printhead.  
  
Mounting of GBP is designed to use adjustable Klicky style mount. This can change in the future, but at this moment allows simple adjustment of the position in the frame.  

# BOM
- 1x Silicone bracelet 12x200
- 1x DC motor 12mm diameter 12V 50rpm (or less) - Commonly sold as GA12 N20 or N30. See note below.
- 2x 623 bearings  
- 2x 6x3 magnet (Voron standard size)  
- 12x heatset insert M3x5x4 (Voron standard size)
   
**M3 hardware:**  
 - 1x M3 nut
 - 1x M3 grub screw
 - 8x M3x8 SHCS
 - 1x M3x16 SHCS
 - 1x M3x20 SHCS
 - 2x M3x20 FHCS  

If your board has option to supply 5V to fan outputs, you may get 6V motor version. If you have just 24V output, get 12V motor and use less than 50% PWM  
Initial tests have shown, that optimal speed under load is 2 - 12rpm, so you may try as low as 15 rpm version. With PWM you can typicaly get to 5-10% of nominal speed, so choose accordingly. 
  
# Build instructions
To be done...  
But really, it is quite simple, you can assemble it based on pictures or look into the CAD data

# Electrical connection
Connect DC motor to any free power output on your board. Fan output is recommended. If you can, consider supplying it with 12V or even 5V to limit motor heating. If not, make sure to compensate with PWM.  
> [!IMPORTANT]
> DC motor is a strong inductive load, which means it will produce large voltage spikes, which unless treated WILL destroy your boards mosfet or even MCU. A sufficiently dimensioned Schottky flyback diode across board output is the bare minimum, but I also recommend adding a RC snubber to motor terminals.
> As for diodes, some boards have them already included (for example BTT Octopus), some rely just on a diode in a mosfet, which is generally not sufficient. Consult your boards schematic, use your best judgement and when in doubt, ask someone.  
> As for RC snubber, I used a 47 Ohm resistor and 10 nF 50V ceramic capacitor. Solder together one leg of both components and then solder other two to motor, one leg to each motor terminal. 47R/10nF proved to sufficiently dampen any spikes while also not influencing too much the PWM regulation response. You can use larger capacitor if you want to be extra safe, but larger the capacity, the worse PWM response you will have. It is not recommended to replace RC snubber with just capacitor, because this will cause resonance and make matters even worse.  
  
I did measure effectivity of described measures with an oscilloscope, but I cannot guarantee there are no fast transients I did not caught (for example when changing PWM duty cycle) and there is not enough test data gathered yet to be 100% sure there are no issues. So be careful and make sure you understand what is going on. 

# Software configuration
A .cfg with macro is available at this moment, see comments in the file and configure to your printer. Future refinements are expected.  

If you are using HappyHare or similar tool, make sure to configure it to not return to last or next gcode position.
  
Once everything is in place, you can call purge routine by running gcode macro `goose_purge VOLUME=###` or `goose_purge LENGTH=###` where  ### is requested purge volume in mm3 or purge length in mm. It is expected this can be called by a slicer after toolchange.  
If both parameters are provided, `LENGTH` has priority. If neither is provided, purge volume defaults to 0 and routine runs without extruding anything. 

# Slicer configuration
Simply put the `goose_purge VOLUME=###` or `goose_purge LENGTH=###` call at the begining of your code at the place where prime line would be. And to tool change GCode, just after `T# ;` command.  
Unfortunately not all slicers support passing the purge volume as parameter. If you use Orca slicer, following should work `goose_purge LENGTH=[flush_length]`. If you use Prusa slicer, you need to hardcode your purge volume. 
  
# Tips and observed behaviour
Extrusion shall be done on a smooth inner side of the bracelet. You can also try extruding on the outer textured side, but adhesion is generaly worse and also you need a bracelet without embossing or print, which is surprisingly difficult to get. Inner surface has excellent adhesion even when it does not directly touches nozzle, while outer surface generaly needs to be pushed really hard into the nozzle - harder than magnets in design can provide.  
  
Adhesion depends a lot on the belt speed. If you have trouble with fillament sticking, try to slow the belt down. As soon part of the fillament started sticking on the belt, you can speed up, because deposited material starts pulling more from the nozzle.  
Part cooling fan can also improve adhesion but this is not included in macro yet. 
  
Inner smooth bracelet side in fact sticks a little bit too much and if you extrude too thin line, you can have issue getting it off the belt. Solution is to run the belt slow enough so that thick solid waste line gets extruded and has time to cool down. This line then usually just falls off the belt on its end.

Included macro splits waste purge line into short "toothpick" sizes. This is intended as a measure to improve waste volume efficiency.  

Initial tests have shown, that the optimal belt speed is between 2 and 12rpm (or about 1,8 - 11mm/s (108 - 660mm/min). Easiest way to measure your speed is to mark a spot on either pulley or idler, take a stopwatch and just measure time it takes to make 10 revolutions.  

# Risks
This is a mod in an early stage of development which has not been extensively tested. There are number of identified risks which can happen, but also a range of unexpected issues. If you run into any issues, make sure you let me know.   
Identified known risks:  
- low lifetime of the motor (mechanical wear)
- overheating of motor
- burning of the belt
- deformation of structure due to thermal loads and tension of the belt
- belt slipping of rollers
- blobbing of your nozzle due to bad adhesion or due to pieces of plastic sticking too well and making their way all the way around back to nozzle
- messing up your prints and/or printer due to uncontrolled fallout of purged lines

Unexpected (but not impossible) risks:
- killing your goose
- burning down your house
- opening a black hole
- anything not mentioned above

# Future tasks and ideas
As the design stands, it should work as a purger, but does not manage anyhow wasted filament. At this moment it just dumps all the waste down into your printer causing a mess. It is up to user to somehow contain it with a bin of its own. This is of course not desirable state and before GBP can be considered for general useage at least a basic reference solution for waste containment needs to be provided. It is expected as time goes users will create their own versions of the waste bin and therefore GBP shall be designed to allow for widest possible range of waste bin designs.  
  
Additional issue that needs to be adressed is waste volum efficiency, especially given that GBP is intended to improve on contemporary designs. As it is, default purge lines produced by GBP can be rather volume inefficient if left to freely fall into the bin. Proposed solution is some kind of shredder or cutter, that will shred the waste into small pellets. No technical solution is considered at this moment.


