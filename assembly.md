# Goose Belt Purger assembly

## Preparation steps
a) Identify, if your board has protective fetures, i.e. schottky flyback diode and RC snubber. If not or you are in doubt, you will have to add them yourself to motor terminals.  
b) Install heatset inserts into **belt_purger-Arm** (1x), **belt_purger-Idler_offseted** (1x), **belt_purger-Mount** (4x) and **belt_purger-Deflector** (2x). If you playn to use a klicky style mount, you can also install inserts in **klickystyle_mount-Klickystyle-joint** (4x)  
c) Remove integrated support of **belt_purger-Deflector**. Grab the small bridging element in the middle with small pliers and remove it with twisting motion. It should easily and cleanly detach.   

## Assembly steps
1. Solder wires to the motor. If your board does not have sufficient protection, solder a schottky flyback diode and/or RC snubber to the motor terminals together with wires  
  **_1a._** Correct polarity of the motor is needed. Output shaft of the motor should be turning CCW (or CW if you printed mirrored version). If you can, test the polarity beforehand. In case the motor spins other way, switch the wires. If you are not adding flyback diode, you can reverse them later on in connector. If you are adding flyback, you will need to revert them directly on the motor.  
  **_1b._** Schottky flyback diode has a set polarity and you need to solder its cathode to the motor terminal where positive voltage (+) wire is connected. Cathode is usually marked with a colour stripe. Other end of diode shall go directly to the other motor terminal. Double check the polarity, because doing this wrong may overload and damage your control board.  
  **_1c._** RC snubber is formed of a capacitor and a resistor. One leg of each component shall be soldered together (in series) and other legs to the motor terminals (one resistor leg to one motor terminal, one capacitor leg to other motor terminal). RC snubber does not have set polarity and it does not matter which way around do you solder it.  
2. Insert the motor to a slot in **belt_purger-Mount**. It should fit tightly, but not deform the part. Depending on your gearbox configuration you may have some play in axial direction. Try to place your motor in such a way, that the face with output shaft  is as flush with mount as possible.  
3. Clamp the motor from top with **belt_purger-Mount_closure**, push through 2x M3x20 FHCS screws, add **belt_purger-Deflector** to bottom and screw it together.  
4. Insert both bearings and one magnet into **belt_purger-Arm**. Polarity of the magnet does not matter. If it is too loose, you may fix it with a drop of glue, but this should generally not be needed. Magnets act as a spring pulling arm away from mount and as such are being pushed into a pocket.  
5. Put M3x16 screw through a bearing and screw on **belt_purger-Idler_offseted**. Idler has to be fully tightened against bearing, but has to move freely. If it drags or locks against arm, put a washer in between bearing and idler.  
6. Put other magnet into mount. Make sure to orient it in such a way, that it will repulse the magnet in Arm, when they are faced aginst each other.  
7. Slide the arm into t motor shaft, through the other bearing. Make sure the arm can freely move up and down and that it gets pushed away from mount by magnets. Secure the arm to mount with M3x20 SHCS screw (do not fully tighten).  
8. Push the M3 nut into a slot in **belt_purger-Pulley** and screw in a grub screw. You may need to push a nut deep inside with a screwdriver.  
9. Push the pulley into the motor shaft. Push it as far as you can, but leave a tiny gap so that the arm is free to move.  
10. Take the wristband, turn it inside out, slightly stretch it by hand and slide it into a pulley and idler. You don't need to put it into any specific position, it will self align.  
11. Test the assembly. Verify, that the motor turns when powered, that the arm can move and that the belt keeps turning without slipping off the pulleys. If everything is ok, you can fix the wires to the mount with a zip-tie.  

## Mount assembly and integration into printer
Applies to a Voron Trident. You may need to adapt it for other printers.  
  
101. Use 8x M3x8 SHCS screws to put together assmbled goose purger unit, **klickystyle_mount-Klickystyle-joint** and **klickystyle_mount-Klickystyle-long**. You can assemble the parts in any position for now.  
102. Use 2x M5x10 screws to fix the assembly to the rear frame profile. Adjust the X axis position as required. It is recommended to push the GBP as far to the side as possible.  
103. Loosen four vertical M3 screws and adjust the Y axis position. Make sure, that the purger is not in collision with the bed, but also that the nozzle can reach the belt. Nozzle should ideally end up right in the middle of the belt*. Fully tighten the vertical screws.
104. Loosen four vertical M3 screws and fully loosen the M3x20 SHCS screw. Adjust the Z axis position so that the nozzle pushes against the belt and pushes arm slightly down. Check it against the nozzle right above idler. Make sure to do this with a cold nozzle!
105. Screw in the M3x20 SHCS screw so that there is a small gap between a belt and a nozzle.

*) Regarding Y position, having the nozzle right in the middle of the belt can prevent some issues with belt being pushed sideways. On the other hand offsetting the nozzle closer to the belt side will distribute the wear better and allow you to reuse worned wristbands by flipping them around. Either way, keep in mind, that the belt will self adjust its position on pulleys and that it is not guaranteed, that the middle of the belt will match the middle of the pulley.  

## Troubleshooting and FAQ
- My wristband shifts partially/fully off the pulleys - this is most likely due to bad tolerance of a bearing holding the idler. Some cheaper bearings allow the idler to by pulled to side by a band tension, which causes this shifting off. If you can, use a better bearing. Otherwise try using **belt_purger-Idler_offsetted+3mm** (TBD)  
- My wristband shifts too far in, towards motor - less common, could be due to arm being deformed. Try using **belt_purger-Idler**
- Wouldn't it fix the belt shifting if one of the pulley were flanged? - Yes and you can use flanged version as a last resort, but keep in mind, that the flange might prevent you from positioning purger far enough forward in Y axis. Flange also increases the belt wear.
