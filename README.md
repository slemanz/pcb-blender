# PCB Blender

## Export the PCB from KiCad

For this, we need to use **pcb2blender** plugin, and make three checks:

1. The solder mask and silkscreen colors (set in File -> Board Setup -> Physical
Stackup).
2. Every footprint needs an associated .step or .wrl model (use Alt+3 to see).
3. The surface finish, also set in Board Setup, determines the pad material:
ENIG gives gold pads and HASL silver ones.

The export itself runs from the toolbar or in Tools -> external plugins ->
Export to Blender (.pcb3d).

## Create and Import the PCB in Blender

Creating a PCB render project starts with File -> New -> General, deleting the
default cube and light with **X**, setting Render Properties -> Render Engine to
Cycles, and saving immediately with File -> Save As into the project folder.

The board then can be imported through File -> Import -> PCB (.pcb3d).

#### Lights

- Shift+A -> Light -> Area: adds an area light, best for soft light.
- Light properties (green bulb icon) -> Power and Size: large size = softer
shadows and reflections.
- R/G: rotate and move the selected light.

#### Camera

- Shift+A -> Camera: adds a camera if the scene has none.
- Numpad 0: camera view.
- Ctrl+Alt+Numpad 0: snaps the camera to the current viewport.

## Render Configurations

All settings are done in Render Properties and Output Properties. If the
computer has a GPU, go to Edit -> Preferences -> System -> Cycles Render
Devices and select CUDA or OptiX, so the GPU will appear as an option.

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
BSDF) for ICs, diodes, LEDs, buttons, connectors, relays, capacitors and
resistors, named by component prefix: IC_, DIO_, LED_, BTN_, CONN_, JST_,
HDR_, RLY_, CAP_, ALU_, RES_ and RTH_.

To use it, go to File -> Append -> pcb_materials.blend -> Material: select the
materials (Ctrl+click for many) and confirm with Append. After that, Material
Properties (red sphere icon) -> Browse Material, assigns the appended material
to the selected object.

Append copies the material into the project, so it can be edited without
changing the library, unlike Link, which keeps it tied to the source file.

To clean unused materials, use File -> Clean Up -> Purge Unused Data.

## Illumination

A three-point setup gives depth to the board. Each light is added with
Shift+A -> Light -> Area (a flat square emitter, the softest option for all
three), and its values are typed in N -> Item (Location in meters, Rotation in
degrees) and in Light properties (green bulb icon), with the board centered at
the origin.

| **Light** | **Location (X, Y, Z)**   | **Rotation (X, Y, Z)** | **Power**  | **Size**  |
|---|---|---|---|---|
| Key   | (-0.25, -0.25, 0.30) | (50, 0, -45)       | 3 W    | 0.3 m |
| Fill  | (0.30, -0.20, 0.25)  | (55, 0, 56)        | 1.2 W  | 0.5 m |
| Rim   | (0.15, 0.30, 0.15)   | (66, 0, 153)       | 2.5 W  | 0.2 m |

## Camera

To place the camera precisely, the viewport is orbited with the numpad, where
Numpad 4/6 turn around the board and Numpad 8/2 change the height, each press
rotating 15 degrees (three presses = 45 degrees). Once the view looks good,
Ctrl+Alt+Numpad 0 moves the camera to that exact viewpoint, and Numpad 0 checks
the framing through the camera. 

Numpad+5 changes between perspective and isometric view. And in Camera
properties -> Lens -> Type.

## Bottom Side

To render the bottom, the board is flipped while the lights and the camera
stay in place, so the same setup works for any board.

1. Shift+C: puts the 3D cursor at the origin (board center).
2. Pivot Point (header, or the . key): 3D Cursor, since the PCB origin sits on
a board corner.
3. Select the PCB and rotate it with R Y 180: the components follow, as they
are children of the PCB.

## Background
