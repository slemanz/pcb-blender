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

#### Lights

- Shift+A -> Light -> Area: adds an area light, best for soft.
- Light properties (green bulb icon) -> Power and Size: large size = softer
shadows and reflections.
- R/G: rotate and move the selected light.
- Object Constraints -> Track To (target: PCB): keeps a light or camera always
aimed at the board.

#### Camera

- Shift+A -> Camera: adds a camera if the scene has none.
- Numpad 0: camera view.
- Ctrl+Alt+Numpad 0: snaps the camera to the current viewport.

## Render Configurations

All settings are did in Render Properties and Output Properties, if the computer
has GPU, goes in Edit -> Preferences -> System -> Cycles Render Devices: select
CUDA or OptiX, so the GPU will appear as an option.

1. Render Properties -> Device: GPU compute.
2. Render Properties -> Sampling -> Render -> Noise Threshold: 0.01 to 0.02 (0.03 for a
faster render) and Max Samples: 5000, with Denoise enabled (and Use GPU).
3. Render Properties -> Film -> Transparent: removes the world background, so
the board is rendered over transparency (if we want).
4. Output Properties -> Format: 1920 x 1080 at 300% (5760 x 3240).
5. Output Properties -> Output: PNG, RGBA, 8 bits (RGBA keeps the transparency).

The render runs with **F12**, and the result is saved through Image -> Save As
in the Render window.

## Materials

The materials/pcb_materials.blend file stores the default materials (Principled
BSDF) for some components.

To use goes in File -> Append -> pcb_materials.blend -> Material: select the
materials (Ctrl+click for many) and confirm with Append. After that,  Material
Properties (red sphere icon) -> Browse Material, assigns the appended material
to the selected object.

Append copies the material into the project, so it can be edited without
changing the library, unlike Link, which keeps it tied to the source file.

To clean unused materials, use File -> Clean Up -> Purge Unused Data.

