Median Filter Example
===============================

This readme file contains these sections:

1. OVERVIEW
2. SOFTWARE TOOLS AND SYSTEM REQUIREMENTS
3. DESIGN FILE HIERARCHY
4. INSTALLATION AND OPERATING INSTRUCTIONS
5. OTHER INFORMATION (OPTIONAL)
6. SUPPORT
7. LICENSE
8. Acknowledgements
9. REVISION HISTORY

## 1. OVERVIEW

This is an optimized implementation of a median filter being used to remove noise in images
targeting execution on an SDAccel supported FPGA acceleration card.

The main algorithm characteristics of this application are
1. 3x3 median filter
2. Input image size of 128 x 128
3. Achieved performance = 0.045ms on image size 128x128

Achieved performance was measured on an Alpha Data ADM-PCI-7v3 card.

## 2. SOFTWARE TOOLS AND SYSTEM REQUIREMENTS
Tested on:
* Alpha Data ADM-PCIE-KU3, Alpha Data ADM-PCIE-7V3
* SDAccel 2016.1

## 3. DESIGN FILE HIERARCHY
Application code is located in the src directory.
Accelerator binary files are located in the xclbin directory. This directory is required by the Makefile. The contents of the directory are generated during compilation.

## 4. INSTALLATION AND OPERATING INSTRUCTIONS
### Compilation for x86 based servers

1. make all
Launches compilation both emulation and board execution modes of the application for all supported boards.
To limit or change the boards for compilation, edit the utility/boards.mk file located at the top level of this repository.

2. Executing emulation flows
Set the following environment variable and create a emconfig.json file
	```
	export XCL_EMULATION_MODE=true
	emconfigutil --xdevice 'xilinx:adm-pcie-ku3:2ddr:2.1' --nd 1
	```
Run the application as
    ```
    ./median_X86.exe data/inputImage.bmp ./xclbin/krnl_median.<emulation type>.xilinx_adm-pcie-ku3_2ddr_2.1.xclbin
    ```
The choices for emulation flow are
    - sw_emu = software emulation
    - hw_emu = hardware emulation

3. Running the application on a board hosted by Nimbix from a remote system
Install the Xilinx Runtime for board in Nimbix using the xbinst utility in SDAccel
    ```
    xbinst -x xilinx:adm-pcie-ku3:2ddr:2.1 -d .
    mv xbinst/pkg/pcie/runtime .
    rm -rf xbinst
    ```
Run the nimbix-run.py script provided in the utility/nimbix directory at the top level of this repository
    ```
    ../../utility/nimbix/nimbix-run.py ./median_X86.exe data/inputImage.bmp ./xclbin/krnl_median.hw.xilinx_adm-pcie-ku3_2ddr_2.1.xclbin
    ```
Only the Alpha Data ADM-PCIE-KU3 card is supported by this method.

4. Running the application from within Nimbix
Follow the instructions in the [Nimbix Getting Started Guide][]

### Compilation for IBM POWER servers
1. View the SuperVessel [Walkthrough Video][]

2. make all-POWER
Launches compilation both emulation and board execution modes of the application for all supported boards.

## 5. OTHER INFORMATION

For more information check here:
[SDAccel User Guides][]

## 6. SUPPORT
For questions and to get help on this project or your own projects, visit the [SDAccel Forums][].

## 7. License
The source for this project is licensed under the [3-Clause BSD License][]

## 8. Acknowledgements
The example is written by developers at [Xilinx](http://www.xilinx.com/)

## 10. REVISION HISTORY

Date    | Readme Version | Revision Description
--------|----------------|-------------------------
FEB2016 | 1.0            | Initial Xilinx release
JUL2016 | 2.0            | Conversion to Makefile based compilation


[3-Clause BSD License]: LICENSE.md
[SDAccel Forums]: https://forums.xilinx.com/t5/SDAccel/bd-p/SDx
[SDAccel User Guides]: http://www.xilinx.com/support/documentation-navigation/development-tools/software-development/sdaccel.html?resultsTablePreSelect=documenttype:SeeAll#documentation
[Nimbix Getting Started Guide]: http://www.xilinx.com/member/sdaccel_ea_40hr/Getting_Started_with_SDAccel_on_Nimbix_cloud.pdf
[Walkthrough Video]: http://bcove.me/6pp0o482