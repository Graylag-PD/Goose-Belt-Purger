# List of boards with or without flyback diodes
This is list of 32bit boards from common vendors with open source documentation and details whether specific board does or does not have protective flyback diodes on its outputs.  
List is created manualy and verified by me personaly based on schematics. If you have any board not listed or do you have any new or conflicting information, let me know.  
  
In some cases it is noted, that 1A Schottky or 1A rectifier diode is used. 1A Schottky is perfectly fine if you have snubber soldered and you use 6V motor at 5V or 12V motor at 12V, but can be a risk for 12V motor at 24V. 
Although this should still work, diode could be damaged over long time, especially if its not properly cooled. Use it at your own risk, or simply add another Schottky directly to motor clamps.  
1A rectifier diode is different story though. Plain rectifier diode is not fast enough to operate at 25 kHz PWM and will be mostly inefficient. You could try to lower your PWM frequency below 1kHz, but it is highly recommended to disregard this diode and add Schottky directly to motor.
  
## BTT
<table>
  <tr>
    <th>Board identification</th>
    <th>Diode on fan outputs?</th>
    <th>Diode on power outputs?</th>
    <th>Note</th>
  </tr>
  <tr>
    <td>Octopus /Pro/Max</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>All versions</td>
  </tr>
  <tr>
    <td>Manta M8P V1/V2</td>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
  </tr>
  <tr>
    <td>Manta M5P V1</td>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
  </tr>
    <tr>
    <td>Manta M4P V2</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>Manta E3EZ V1</td>
    <td>Yes</td>
    <td>No</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>SKR Pico V1</td>
    <td>Yes</td>
    <td>No</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>SKRat V1</td>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
  </tr>
  <tr>
    <td>Kraken V1</td>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR 3 /EZ</td>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR V1.1/1.3/1.4/2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR mini V1.1</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR Pro V1.1/1.2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR MINI E3 V3</td>
    <td>see note</td>
    <td>see note</td>
    <td>1A rectifier diode</td>
  </tr>
  <tr>
    <td>SKR MINI E3 V1/V1.2/V2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>E3 RRF</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>SKR E3 DIP/Turbo</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>GTR</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
</table>

## Fysetc
<table>
  <tr>
    <th>Board identification</th>
    <th>Diode on fan outputs?</th>
    <th>Diode on power outputs?</th>
    <th>Note</th>
  </tr>
  <tr>
    <td>Spider King</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Diode type not specified, presumed to by 1A Schottky</td>
  </tr>
  <tr>
    <td>Spider Pro</td>
    <td>Yes</td>
    <td>No</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>Spider V2.3/V3/V3 H7</td>
    <td>Yes</td>
    <td>No</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>Spider V1/V2.2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Cheetah V1/2/3</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Cheetah Mix</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Catalyst Main Board</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>S6</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>R4</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>1A Schottky</td>
  </tr> 
  <tr>
    <td>R4</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>1A Schottky</td>
  </tr> 
</table>

## Mellow
Mellow documentation can be difficult to understand. For some boards they drawed some diodes, but without any additional details. It is recommended to treat those boards as NOT protected unless confirmed otherwise.
<table>
  <tr>
    <th>Board identification</th>
    <th>Diode on fan outputs?</th>
    <th>Diode on power outputs?</th>
    <th>Note</th>
  </tr>
  <tr>
    <td>Fly Super8 /Pro</td>
    <td>Yes?</td>
    <td>Yes?</td>
    <td>Unclear documentation</td>
  </tr>
  <tr>
    <td>Fly Super5Pro</td>
    <td>Yes?</td>
    <td>Yes?</td>
    <td>Unclear documentation</td>
  </tr>
  <tr>
    <td>Fly Gemini V1/1.1/2/3</td>
    <td>Yes?</td>
    <td>Yes?</td>
    <td>Unclear documentation</td>
  </tr>
  <tr>
    <td>Fly E3 V2</td>
    <td>Yes?</td>
    <td>Yes?</td>
    <td>Unclear documentation</td>
  </tr>
  <tr>
    <td>Fly RRF E3</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Fly E3 Pro V1/2/3</td>
    <td>No?</td>
    <td>No?</td>
    <td>Unclear documentation</td>
  </tr>
  <tr>
    <td>Fly Puppet</td>
    <td>Yes</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Fly CDY V3</td>
    <td>Yes?</td>
    <td>Yes?</td>
    <td>Diode type not specified, could be rectifier diode</td>
  </tr>
  <tr>
    <td>Fly CDY V1/2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>Fly Mini</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
</table>

## MKS
<table>
  <tr>
    <th>Board identification</th>
    <th>Diode on fan outputs?</th>
    <th>Diode on power outputs?</th>
    <th>Note</th>
  </tr>
  <tr>
    <td>Monster8 V1/V2</td>
    <td>see note</td>
    <td>see note</td>
    <td>1A rectifier diode</td>
  </tr>
  <tr>
    <td>OWL V1</td>
    <td>see note</td>
    <td>see note</td>
    <td>Diode type not documented, presumed to be 1A rectifier diode</td>
  </tr>
  <tr>
    <td>SKIPR V1</td>
    <td>see note</td>
    <td>see note</td>
    <td>Diode type not documented, presumed to be 1A rectifier diode</td>
  </tr>
  <tr>
    <td>Robin (All versions)</td>
    <td>No</td>
    <td>No</td>
    <td>Includes Robin, Robin2, Pro, Mini, Nano1/2/3, Lite1/2/3, E3/E3D</td>
  </tr>
  <tr>
    <td>Eagle</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
  <tr>
    <td>TinyBee</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
    <tr>
    <td>SBASE/SGEN</td>
    <td>No</td>
    <td>No</td>
    <td>Includes SGEN_L V1/2 </td>
  </tr>

</table>

## LDO
<table>
  <tr>
    <th>Board identification</th>
    <th>Diode on fan outputs?</th>
    <th>Diode on power outputs?</th>
    <th>Note</th>
  </tr>
  <tr>
    <td>Leviathan V1.3</td>
    <td>Yes</td>
    <td>No</td>
    <td>1A Schottky</td>
  </tr>
  <tr>
    <td>Leviathan V1.2</td>
    <td>No</td>
    <td>No</td>
    <td></td>
  </tr>
</table>


</table>
