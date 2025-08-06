# Configuration file and macro configuration

## Instalation of macro into Klipper
Copy the `goose_belt.cfg` file into Klipper configuration file and add `[include goose_belt.cfg]` into your `printer.cfg`. No additional steps are needed

## Macro configuration
Macro can be configured by changing its user accessible variables. See comments in the file and adapt it to your printer.  

Once set up, the purge routine can be triggered by running the G-code macro:  
 - goose_purge PURGE_VOLUME=###  
   or  
 - goose_purge PURGE_LENGTH=###  
where ### is the purge volume in mm³ or length in mm. If both are provided, PURGE_LENGTH takes precedence. If neither is provided, the macro runs according default value variable configured within the macro.

## Useful tips
As for tuning, I set up a test print that has a bunch of filament changes, then adjusted the variables live as I went. I used the command SET_GCODE_VARIABLE MACRO=goose_purge VARIABLE=belt_pwm VALUE=0.33 for example to change the belt speed. Because klipper queues up gocode, it doesn't take effect immediately so if you enter this while you're in the middle of a toolchange, you won't see the results until the next toolchange. This doesn't save the values, though so you'll have to remember to set the values in the cfg file to save them. It's easier than doing a save_config between each change. 

## GBP and other printer firmwares
No other printer firmwares are supported as of this moment. We are open to add support for other firmwares if you are interested, however be prepared to provide extensive support for a development for your choosen platform.

# Integration of Goose Belt Purger into your print jobs

## Standalone operation (Slicer controlled)
Place the goose_purge macro at the beginning of your start G-code where you'd normally place a prime line. For tool changes, insert it just after the T# ; command.  

Note that not all slicers support passing purge parameters. In OrcaSlicer, you can use goose_purge PURGE_LENGTH=[flush_length]. In PrusaSlicer, the volume must be hardcoded.  

If you're using HappyHare or a similar tool, configure it so it doesn't return to the previous or next G-code position.  

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

## Happy Hare integration

TBD

## Other filament management add-ons
No other filament management add-ons are supported as of this moment. You are free to develop integration into other add-ons and if you do, we can list them here, however be prepared to support such solution. 

