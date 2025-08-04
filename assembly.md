# Goose Belt Purger assembly

## Preparation steps
a) Identify, if your board has protective flyback diodes. You can check the comprehensive list [here](board_list.md),. If not or you are in doubt, you will have to add it yourself to motor terminals. At this moment there are no known boards with RC snubber integrated, so expect to add snubber either way.
  
![](/Assets/Schottky-Octopus.jpg)  
_Example of BTT Octopus with Schottky diode on board_  
  
b) Install heatset inserts into **GBP_Arm** (1x), **GBP_Idler(+#)** (1x) and **GBP_Deflector_(long/short)** (2x). If you plan to use an AT style mount, you can also install inserts in **mount_upper** (2x)  
c) Remove integrated support of **GBP_Deflector_(long/short)**. Grab the small bridging element in the middle with small pliers and remove it with twisting motion. It should easily and cleanly detach.   

## Assembly steps
1. Solder wires to the motor. If your board does not have sufficient protection, solder a schottky flyback diode and/or RC snubber to the motor terminals together with wires  
  **_1a._** Correct polarity of the motor is needed. Output shaft of the motor should be turning CCW (or CW if you printed mirrored version). If you can, test the polarity beforehand. In case the motor spins other way, switch the wires. If you are not adding flyback diode, you can reverse them later on in connector. If you are adding flyback, you will need to revert them directly on the motor.  
  **_1b._** Schottky flyback diode has a set polarity and you need to solder its cathode to the motor terminal where positive voltage (+) wire is connected. Cathode is usually marked with a colour stripe. Other end of diode shall go directly to the other motor terminal. Double check the polarity, because doing this wrong may overload and damage your control board.  
  **_1c._** RC snubber is formed of a capacitor and a resistor. One leg of each component shall be soldered together (in series) and other legs to the motor terminals (one resistor leg to one motor terminal, one capacitor leg to other motor terminal). RC snubber does not have set polarity and it does not matter which way around do you solder it.  
   
![](/Assets/snubber.jpg)  
_Example of snubber_
   
2. Insert the motor to a slot in **GBP_Frame**. It should fit tightly, but not deform the part. Depending on your gearbox configuration you may have some play in axial direction. Try to place your motor in such a way, that the face with output shaft  is as flush with frame as possible.  
3. Clamp the motor from top with **GBP_Motor_cover**, push through 2x M3x20 FHCS screws, add **GBP_Deflector_(long/short)** to bottom and screw it together.  
4. Insert two of the bearings and one magnet into **GBP_Arm**. Bearings should be inserted from flat side (one with logo on it). Polarity of the magnet does not matter. If it is too loose, you may fix it with a drop of glue, but this should generally not be needed. Magnets act as a spring pulling arm away from mount and as such are being pushed into a pocket.  
5. Put M3x30 screw through remaining bearing, add **GBP_Bearing_spacer** on screw, then push the screw through arm opening and second bearing. Set the third bearing flush into hole and screw on **GBP_Idler(+#)**. Idler has to be fully tightened against bearing, but has to move freely. Do not overtighten, as it can damage your bearings.    
6. Put other magnet into Frame. Make sure to orient it in such a way, that it will repulse the magnet in Arm, when they are faced aginst each other.  
7. Slide the arm on the motor shaft, through remaining bearing. Make sure the arm can freely move up and down and that it gets pushed away from frame by magnets. Secure the arm to mount with M3x16 SHCS screw (do not fully tighten).  
8. Push the M3 nut into a slot in **GBP_Motor_pulley** and screw in a set screw. You may need to push a nut deep inside with a screwdriver.  
9. Push the pulley into the motor shaft. Push it as far as you can, but leave a tiny gap so that the arm is free to move. Tighten the set screw against motor shaft.  
10. Take the wristband, turn it inside out, slightly stretch it by hand and slide it on the pulley and idler. You don't need to put it into any specific position, it will self align when switched on.  
11. Test the assembly. Verify, that the motor turns when powered, that the arm can move and that the belt keeps turning without slipping off the pulleys. 

## Mount assembly and integration into printer
Purger is designed to be compatible with ArmoredTurtle brush mount and regarding its assembly, you should refer to AT's wonderful manual located [**here**](https://www.armoredturtle.xyz/manual.html?manual=at_brush&step=1)  
Keep in mind, if you have a Voron Trident or comparable printer, you will need to use modified parts provided in this repo, to shorten the horizontal reach of AT mount by 10 mm.  
  
Once you have your AT mount assembled, mount it to your printer to desired location and attach the Goose Belt Purger to it. If your printer is properly tuned, GBP should mate with AT mount firmly, without any residual movement, but also without need for excesive force to mate them. If they are too hard to assemble, you can gently sand the sides of dovetail wedges on frame. If they are too loose, you can shim the purger with a piece of paper or thin plastic placed in between purger's frame and AT mount.  
Once everything is in place, you can adjust it to right position using screws in AT mount. Horizontal adjustement screw gets exposed when you remove M3x16 SHCS screw and lift the arm up. Don't forget to put this screw back once you are done.  
You should adjust the Z position in such a way, that there is a tiny gap between nozzle and wristband. You can use the M3x16 SHCS screw for fine adjustement.  
  
If you decide to use Klicky style mount or other kind of mount, assemble and tune it in simillar manner.  

## Troubleshooting and FAQ
- My wristband shifts partially/fully off the pulleys towards bed - this is most likely due to bad tolerance of a bearings holding the idler and/or missing M3x16 SHCS screw. Some cheaper bearings allow the idler or arm to be pulled to the side by a band tension, which causes this shifting off. If you can, use a better bearing. Otherwise try using **GBP_Idler(+3,0)**  
- My wristband shifts too far in, towards motor - less common, could be due to arm being warped. Try using **GBP_Idler(+0)**



