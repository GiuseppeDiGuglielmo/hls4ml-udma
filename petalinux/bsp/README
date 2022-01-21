# Getting a BSP for Ultra96-V2

A BSP for Ultra96-V2 for **Petalinux 2019.1** should be available on the [Avnet webpage](https://www.avnet.com/wps/portal/us/products/avnet-boards/avnet-board-families/ultra96-v2) under Reference Designs. If the links are broken and you cannot find a BSP online, the you can either download it from [this Google Drive](https://drive.google.com/file/d/1C58ABiOUSS5vGUXMKSlVV8t5wrrCHClT/view?usp=sharing) or build one from scratch.

More BSPs are also available [here](https://avtinc.sharepoint.com/teams/ET-Downloads/Shared%20Documents/Forms/AllItems.aspx?id=%2Fteams%2FET%2DDownloads%2FShared%20Documents%2Fprojects%2Fpublic%5Frelease), but they may be for more recent version of Petalinux / Xilinx suites.

## Build from Scratch

Follow these instructions and then place the generated BSP in the current directory.
```
git clone -b 2019.1 https://github.com/Avnet/hdl.git
git clone -b 2019.1 https://github.com/Avnet/petalinux.git
git clone https://github.com/Avnet/bdf.git
cd ./petalinux/scripts
source ./make_ultra96v2_obb_bsp.sh
ls ./petalinux/projects/ultra96v2_oob_2019_1.bsp
```
