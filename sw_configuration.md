# Configuration file and macro configuration
Default control software is the [Goose Purge Macro](https://github.com/Graylag-PD/Goose-Purge-Macro). For details on installation, see there.  
Information in this file apply to macro version v0.8.2 or newer. If you have any kind of issues, check first, if you are on newest available version.  
  
The old version of the macro (v0.7.3) remains within the Goose Belt Purger repository and is now considered a legacy solution. 

## Macro configuration
Macro can be configured by changing the variables in `goose_purge.cfg` file. See comments in the file and adapt it to your printer.  

Once set up, the purge routine can be triggered by running the G-code macro:  
 - `GOOSE_PURGE PURGE_VOLUME=###`  
   or  
 - `GOOSE_PURGE PURGE_LENGTH=###`  
where ### is the purge volume in mm³ or length in mm. If both are provided, they are added up. If neither is provided, the macro runs according default value variable configured within the macro.

Below are the most common "chokepoints" for the new users:  
### Output pin definition
This part is usually straight forward and the only difficulty may come from identifying correct MCU pin(s). For this you need to consult your board documenatation.  
  
For a DC motor, use a fan output. There is hovewer a catch - although any fan output can be used, or in fact any output pin, not all pins are capable of a HW PWM and this may apply even to pins which should be HW PWM capable from chip design. 
This is not a bug, but comes from the way Klipper is configured. If you run into situation, that everything is connected and configured correctly but the motor still does not activates, consider changing the output which you are using.  
If for whatever reason you cannot use any other port, you may fall back to using the SW generated PWM, but be aware of the performance penalty this can mean.   
One example of the pin which should work but does not is the PA8 on a STM32F446.
  
### Z axis strategy
Applicable Z axis strategy depends on the purger mounting and printer type. For a Voron Trident (or similar) printers you should use "0" - no Z movement. For a Voron 2.4 and purger on the gantry you can use either strategy "0" or strategy "2". Later can help with pellets pilling up under the purger.  
For a Voron 2.4 with the purger fixed on a frame, use strategy "1" - absolute Z position.
  
### Staging and purging positions
At first you should think about your purging position. In general you have two options - either purge directly on top of your idler pulley, or to purge on a free belt span (in between pulleys). If you are in doubt, start with purging position above the idler belt, as the pressure from the idler creates more stable conditions and it maximises the cooling time for the deposited material.  
Purging on a free belt span is often used by more experienced users who aim to maximise purge thickness (to create short, thick pellets), as the belt flexes and adapts better to thick extrusions.  
  
Once you decide on your purging position, you can define your staging position. In general your staging can be anywhere, as its only purpose is to have defined safe position from which we can reach the purging position. But in practice you need to consider oozing and as such you should have your staging position as close to purging position as possible AND have it so, that by moving to purging position you wipe away any ooze. This usually means perpendiculary to the belt or between motor pulley and purging position. If you do this wrong, the ooze might be squished to the side of the nozzle where it sticks and later on causes issues.    

### Motor PWM (belt_pwm variable)
Although this parameter should be easy to configure, unfortunately there is a great variability between motors which you can buy. Some n20 motors are built for a high torque and spin well even with very low PWM duty cycles, while some motors are built for a lower torque and need considerable higher PWM. This can be most easily identified by the static coil resistance, but unfortunately this parameter is usually listed in the item description.  
Fortunately there is a simple way to determine the necessary values. Connect your (fully assembled) purger to the printer, configure the macro with a default values and restart the klipper. As soon the printer comes back online, you should see the purger in your interface (Mainsail or FLuidd) along the fans with option to control its speed with a slider. This way you can quickly try out various PWM settings without actually running the macro.  
When testing the motor, you are looking at three different thresholds. First one is the minimum start-up speed - this is the PWM value at which the motor reliably starts even from standstill. This is your safe value which you should ideally put to your macro. Next up is the minimum stable speed - this is the PWM value, at which the motor can still run reliably if kickstarted. This is usually 10% below minimum start-up speed and you can use it with correctly set kickstart paramerer. Keep in mind, that the purging itself is loading the motor and can stall it if you go too low.  
Third value is the absolute lowest speed - this is the speed, at which the motor is still able to run, but tends to stall at any kind of load. This is usually 5% below minimum stable speed. There is no practical use for this setup.  
  
Once you find the value you are happy with, set it to the macro variables, restart the klipper and do the test run of the macro just to see, that everything works as expected. 

## Useful tips
Tuning the variables "just right" can take many iterations and hitting that Save&Restart button every time can be very time consuming. Instead, you can edit your variables on the fly through terminal command `SET_GCODE_VARIABLE`, e.g. `SET_GCODE_VARIABLE MACRO=_GOOSE_PURGE_VARIABLES VARIABLE=belt_pwm VALUE=0.33` to change the belt speed. You can even do that during print job - set up a test print that has a bunch of filament changes, then adjusted the variables live as you go. Because the way klipper queues up gocode, it may not take effect immediately so if you enter this while you're in the middle of a toolchange, you won't see the results until the next toolchange.  
Keep in mind this doesn't save the values to config file, so you will have to remember to set the values in the cfg file yourself. 

## GBP and other printer firmwares
No other printer firmwares are supported as of this moment. We are open to add support for other firmwares if you are interested, however be prepared to provide extensive support for a development for your choosen platform.

# Integration of Goose Belt Purger into your print jobs

## Standalone operation (Slicer controlled)
You can use GBP in standalone mode independently on any other klipper add-ons. You can even use it this way if you don't have any MMU/AMS on your printer, e.g. for manual filament changes. Purging can be triggered manualy, or more commonly by slicer.  

Slicer configuration is following:  
For initial priming, place the `GOOSE_PURGE` command with appropriate parameter at the beginning of your **Start G-code** where you'd normally place a prime line.  
For tool changes, insert the command into **Tool change G-code** just after the `T#` command.  
Note that not all slicers support passing purge parameters. In OrcaSlicer, you can use `GOOSE_PURGE PURGE_LENGTH=[flush_length]`. In PrusaSlicer, the volume must be hardcoded.  For other slicers, consult their documentation. 

If you are using GBP in standalone mode but let filament changes handling to Happy Hare or a similar tool, configure it so it doesn't return toolhead to the previous or next G-code position to avoid unnecessary travel.  

## AFC integration 
*Contributed in full by adamcstorm*  
To integrate with AFC-Klipper-Add-On:  

Modify `AFC.cfg`. Under the Macro Config section, change the poop settings to use the goose_purge macro, and disable the kick option:  
```
# Poop Settings
poop: True                      # Enable Poop.
poop_cmd: GOOSE_PURGE           # Poop macro name.

# Kick Settings
kick: False                     # Enable Kick.
kick_cmd: AFC_KICK              # Kick macro name.
```
All other settings can be enabled/disabled per your preference.  
Of note, if you have wipe enabled, the wipe routine will be called twice. This is due to the default AFC order of operation that wipes both before and after the kick routine. Since we have disabled the kick routine, it performs the wipe twice in a row. It is recommended to disable the wipe option in `AFC.cfg`, and instead call the brush routine within the `goose_purge` macro by modifying the user_end_script variable: `variable_user_end_script: 'AFC_BRUSH'`.

GBP is compatible with the default AFC method of passing the variable purge length from the slicer to the print via gcode. To configue this, follow this guide:  
https://www.armoredturtle.xyz/docs/afc-klipper-add-on/features.html?h=variable+purge#variable-purge-length-on-filament-change.  
Be aware, that the GBP macro does not access the AFC variables and as such ignores the AFC's variable `variable_purge_lenght`. Instead you should configure GBP's own variable `variable_default_purge_volume`. This value gets used only when no purge parameters are provided, such as during initial load. 

## Happy Hare integration
Happy Hare takes rather complex approach to purging. Unlike other methods it does not rely on slicer to pass purge volume or length together with toolchange. Instead it works with internal purge volume matrix and further enhances it by adding volume of residual filament and cut tip fragment. It can get its purge volume matrix either from internal toolmap or from processing gcode metadata. Either way, for purging to work you need to make sure, that your instance of Happy Hare processes this data correctly. For more information, please visit Happy Hare documentation here:  
https://github.com/moggieuk/Happy-Hare/wiki/Tip-Forming-and-Purging

Also please note, that even if your Happy Hare is configured correctly, it relies on slicer to pass the purging matrix correctly.  

As for actual integration, this is very straightforward and the only thing you need to do is edit `mmu_parameters.cfg` by setting handle `purge_macro: '_goose_purge_hh'` without any parameters.  
The only thing to keep in mind is that the `purge_macro:` handle is case sensitive, so you need to watch your capitalization.
  
Note, that we are not calling directly `GOOSE_PURGE` macro, but instead its wrapper `_goose_purge_hh`. This is due to how Klipper resolves macros and passes parameters.
  
Happy Hare does not have any wiping logic embedded, so if you want automatic wiping execution after purge, consider adding your custom wiping macro to the `variable_user_end_script:''` in `goose_purge.cfg`.

## Other filament management add-ons
No other filament management add-ons are supported as of this moment. You are free to develop integration into other add-ons and if you do, we can list them here, however be prepared to support such solution. 

# Advanced Slicer configuration
Following paragraphs are not mandatory for the purger operation, however may improve your experience and print results.  

## Purger operation together with a prime tower (Orca Slicer)
Although the Goose Belt Purger can completely eliminate purge and prime towers, going to the print object directly after purging, some users report issues with oozing debris contaminating the print and prefer to use a small prime tower along the purger. This means they do the purging on the GBP, but then follow up with the prime tower, where they catch any possible debris and stabilize the nozzle pressure.  
The Orca Slicer has a provision for this kind of a setup - to enable this, you need to enable the prime tower in the global setting, but disable the **Purge in prime tower** in the Printer settings.   
