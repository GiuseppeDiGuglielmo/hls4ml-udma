#BOARD=zcu106
#VER=2019.1
#BSP := xilinx-$(BOARD)-v$(VER)-final.bsp

BSP := ultra96v2_oob_2019_1.bsp

download:
ifeq ("$(wildcard $(BSP))","")
#	@echo "Please download $(BOARD) BSP (prod-silicon) $(VER) and place the file $(BSP) in this folder. Autentication is required."
#	@echo "  https://www.xilinx.com/member/forms/download/xef.html?filename=$(BSP)&akdm=1"
	@echo "Download $(BSP) from the link: https://drive.google.com/file/d/1C58ABiOUSS5vGUXMKSlVV8t5wrrCHClT/view?usp=sharing"
	@echo "If download fails please follow the instructions in README.md"
else
	@echo "File $(BSP) available."
endif

.PHONY: download
