# FPGA Raycaster
A Wolfenstein 3D style raycasting engine, written entirely in LabVIEW FPGA.

Note that the LabVIEW FPGA module or FPGA hardware isn't a requirement - the same code will run on PC using LabVIEW, including LabVIEW Community edition.

Click the image below to see a short video of it in action.
[![FPGA Raycaster - Click for video](images/fpga-raycaster-setup.jpg?raw=true)](https://www.youtube.com/watch?v=pskTGOPSQUQ "FPGA Raycaster - Click for video")

## Technical Details
* Runs entirely from cRIO-9041 FPGA (a Xilinx Kintex-7 7K70T), no RT required
    * Also runs on PC without code changes
* 800x600@60Hz video
    * 2-bit R-2R circuit provides VGA signal
* 400x300 internal render resolution, pixel doubled
* Double buffered video
* 64 color palette on FPGA, 256 colors on desktop (via intensity graph)
* Parallelized column renderer
* 30 fps on FPGA, 60 fps on PC
    * 60 fps on FPGA with 4x parallelization and no sprites (frees up DSP space needed)
* Includes [FizzleFade](https://fabiensanglard.net/fizzlefade/index.php)!

## Video Adapter
A simple circuit was created as a video adapter from the 5V TTL module.

![Video adapter](images/video-adapter.jpg?raw=true)

There's two bits per color component, each converted to the required 0.7Vpp VGA levels using an R-2R circuit. Signals and schematic are below.

Signal          | NI-9401 Pin | Resistor | VGA Pin
----------------|-------------|----------|--------
DO0 (Red LSB)   | 14          | 750      | 1
COM             | 3           | 100      | 1
DO1 (Red MSB)   | 16          | 390      | 1
DO2 (Green LSB) | 17          | 750      | 2
COM             | 6           | 100      | 2
DO3 (Green MSB) | 19          | 390      | 2
DO4 (Blue LSB)  | 20          | 750      | 3
COM             | 9           | 100      | 3
DO5 (Blue MSB)  | 22          | 390      | 3
DO6 (H_Sync)    | 23          | -        | 13
DO7 (V_Sync)    | 25          | -        | 14
COM (GND)       | 10          | -        | 5
COM (Sync GND)  | 10          | -        | 10
COM (Red GND)   | 1           | -        | 6
COM (Green GND) | 1           | -        | 7
COM (Blue GND)  | 1           | -        | 8

![Video adapter schematic](images/9401-to-vga-schematic.png?raw=true)

The two bits per color allow for up to 64 colors in total. The image below shows a side-by-side of a 64 color image, and the resulting output from the video adapter. The colors aren't great compared to the source image, but for a couple of dollars' worth of video adapter, they aren't bad.

![Video adapter](images/64-color-palette-side-by-side.jpg?raw=true)

## Acknowledgements
This project includes modified graphics and maps from shareware version of Wolfenstein 3D.
Copyright (c) id Software

Includes h_sync and v_sync functions from [Tetris On FPGA](https://github.com/VITechnologies/TetrisOnFPGA), used under the MIT license.
Copyright (c) 2019 VI Technologies

Portions of this code adapted from examples found in [Game Engine Black Book: Wolfenstein 3D](https://fabiensanglard.net/gebb/index.html) by Fabien Sanglard.

Portions of this code adapted from examples provided in the series of [raycasting articles](https://lodev.org/cgtutor/raycasting.html) by Lode Vandevenne.
