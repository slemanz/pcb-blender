# PCB Blender

## Export the PCB from Kicad

For this, we need to use **pcb2blender** plugin, and make three checks:

1. The solder mask and silkscreen colors (set in File -> Board Setup -> Physical
Stackup).
2. Every footprint needs an associated .step or .wrl model (uses Alt+3 to see).
3. The surface finish, also set in Board Setup, determines the pad material:
ENIG gives gold pads and HASL silver ones.

The export itself runs from the toolbar or in Tools -> external plugins ->
Export to Blender (.pcb3.). 

## Create and Import the PCB in Blender

Creating a PCB render project starts with File -> New -> General, deleting the
default cube and light with **X**, setting Render Properties -> Render Engine to
Cycles, and saving immediatealy with File -> Save As into the project folder.

The board then can be imported through File -> Import -> PCB (.pcb3d). 

