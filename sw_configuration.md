# Configuration file and macro configuration
Applies to configuration file v0.7.1. If you have any kind of issues, check first, if you are on newest available version.

## Instalation of macro into Klipper
Copy the `goose_belt.cfg` file into Klipper configuration file and add `[include goose_belt.cfg]` into your `printer.cfg`. No additional steps are needed

## Macro configuration
Macro can be configured by changing its user accessible variables. See comments in the file and adapt it to your printer.  

Once set up, the purge routine can be triggered by running the G-code macro:  
 - `GOOSE_PURGE PURGE_VOLUME=###`  
   or  
 - `GOOSE_PURGE PURGE_LENGTH=###`  
where ### is the purge volume in mm³ or length in mm. If both are provided, PURGE_LENGTH takes precedence. If neither is provided, the macro runs according default value variable configured within the macro.

## Useful tips
Tuning the variables "just right" can take many iterations and hitting that Save&Restart button every time can be very time consuming. Instead, you can edit your variables on the fly through terminal command `SET_GCODE_VARIABLE`, e.g. `SET_GCODE_VARIABLE MACRO=goose_purge VARIABLE=belt_pwm VALUE=0.33` to change the belt speed. You can even do that during print job - set up a test print that has a bunch of filament changes, then adjusted the variables live as you go. Because the way klipper queues up gocode, it may not take effect immediately so if you enter this while you're in the middle of a toolchange, you won't see the results until the next toolchange.  
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

## Happy Hare integration
Happy Hare takes rather complex approach to purging. Unlike other methods it does not rely on slicer to pass purge volume or length together with toolchange. Instead it works with internal purge volume matrix and further enhances it by adding colume of residual filament and cut tip fragment. It can get its purge volume matrix either from internal toolmap or from processing gcode metadata. Either way, for purging to work you need to make sure, that your instance of Happy Hare processes this data correctly. For more information, please visit Happy Hare documentation here:  
https://github.com/moggieuk/Happy-Hare/wiki/Tip-Forming-and-Purging

Also please note, that even if your Happy Hare is configured correctly, it relies on slicer to pass the purging matrix correctly. While this assumption seems to be met for Prusa Slicer, it does not seem so for Orca Slicer. If you use Orca Slicer, consider generating purge volume matrix from toolmap, using custom purge volume matrix or operating GBP in standalone mode.  

As for actual integration, this is very straightforward and the only thing you need to do is edit `mmu_macro_vars.cfg` by setting variable `variable_user_post_load_extension : '_GOOSE_PURGE_HH'` without any parameters.   
  
Note, that we are not calling directly `GOOSE_PURGE` macro, but instead its wrapper `_GOOSE_PURGE_HH`. This is due to how Klipper resolves macros and passes parameters.
  
Happy Hare does not have any wiping logic embedded, so if you want automatic wiping execution after purge, consider adding your custom wiping macro to the `variable_user_end_script:'` in `goose_belt.cfg`.

## Other filament management add-ons
No other filament management add-ons are supported as of this moment. You are free to develop integration into other add-ons and if you do, we can list them here, however be prepared to support such solution. 

