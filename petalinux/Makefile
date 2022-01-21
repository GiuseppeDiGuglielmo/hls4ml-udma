BSP := ultra96v2_oob_2019_1.bsp
PROJECT_DIR := ultra96v2_oob_2019_1

HDF_DIR := ../../sdk

BITSTREAM_FILE := core_design_wrapper.bit

#TARGET_IP ?= zynqmp03.cs.columbia.edu

all:
	@echo "INFO: Disabled"

$(PROJECT_DIR):
	petalinux-create -t project -s bsp/$(BSP)

create-project: $(PROJECT_DIR)
.PHONY: create-project

configure-project: create-project
ifneq ("$(wildcard $(HDF_DIR)/*hdf)",)
	cd $(PROJECT_DIR) && petalinux-config --get-hw-description=$(HDF_DIR) --silentconfig
else
	@echo "WARNING: There is not *.hdf file in $(HDF_DIR)"
endif
.PHONY: configure-project

build-project: configure-project
	cd $(PROJECT_DIR) && petalinux-build
.PHONY: build-project

create-image:
	cd $(PROJECT_DIR) && petalinux-package --boot --format BIN --fsbl images/linux/zynqmp_fsbl.elf --fpga images/linux/system.bit --u-boot images/linux/u-boot.elf --pmufw images/linux/pmufw.elf --force
.PHONY: create-image

cp-image:
	cp $(PROJECT_DIR)/images/linux/BOOT.BIN /media/$(USER)/boot
	cp $(PROJECT_DIR)/images/linux/image.ub /media/$(USER)/boot
.PHONY: cp-image

#scp-image:
#	scp $(PROJECT_DIR)/images/linux/BOOT.BIN root@$(TARGET_IP):/boot
#	scp $(PROJECT_DIR)/images/linux/image.ub root@$(TARGET_IP):/boot
#.PHONY: scp-image

distclean:
	rm -rf $(PROJECT_DIR)
.PHONY: distclean

