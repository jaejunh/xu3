#
# Makefile for the linux kernel.
#
ccflags-$(CONFIG_ARCH_MULTIPLATFORM) := -I$(srctree)/$(src)/include \
	-I$(srctree)/arch/arm/plat-versatile/include

obj-y					:= v2m.o
obj-$(CONFIG_ARCH_VEXPRESS_CA9X4)	+= ct-ca9x4.o
obj-$(CONFIG_ARCH_VEXPRESS_DCSCB)	+= dcscb.o	dcscb_setup.o
obj-$(CONFIG_ARCH_VEXPRESS_TC2)		+= tc2_pm.o tc2_pm_setup.o
ifeq ($(CONFIG_ARCH_VEXPRESS_TC2),y)
obj-$(CONFIG_ARM_PSCI)			+= tc2_pm_psci.o
CFLAGS_REMOVE_tc2_pm_psci.o		= -pg
endif
obj-$(CONFIG_SMP)			+= platsmp.o
obj-$(CONFIG_HOTPLUG_CPU)		+= hotplug.o
