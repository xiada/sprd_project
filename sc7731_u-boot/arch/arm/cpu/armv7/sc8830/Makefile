#
# (C) Copyright 2000-2006
# Wolfgang Denk, DENX Software Engineering, wd@denx.de.
#
# See file CREDITS for list of people who contributed to this
# project.
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License as
# published by the Free Software Foundation; either version 2 of
# the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston,
# MA 02111-1307 USA

include $(TOPDIR)/config.mk

LIB	= $(obj)lib$(SOC).o

COBJS	= generic.o timer.o reset.o ldo.o mfp_sprd.o chg_phy.o mmu.o adc.o rtc-sprd.o sec_boot.o glb.o adi.o gpio.o gpio_phy.o clock.o clock_test.o check_reboot.o eic.o

ifdef CONFIG_ADIE_SC2723S
COBJS += vibrator_sc2723.o
else
COBJS += vibrator_sc2713.o
endif

SOBJS += mmu_asm.o
ifndef CONFIG_SPX30G
COBJS += chip_phy_v3.o
endif

ifneq ($(CONFIG_SPX30G)$(CONFIG_SPX35L)$(CONFIG_ADIE_SC2723)$(CONFIG_ADIE_SC2723S),)
COBJS += otp-helper.o efuse.o
ifneq ($(CONFIG_ADIE_SC2723)$(CONFIG_ADIE_SC2723S),)
COBJS += ana_efuse.o
endif

else
COBJS += efuse_drv.o
endif

ifndef CONFIG_SKIP_LOWLEVEL_INIT
COBJS   += lowlevel_init.o sdram.o remap.o sdram_prod.o mmu.o chip_cfg.o mcu.o
SOBJS   = cpu_spec.o mmu_asm.o
endif

ifdef CONFIG_HW_WATCHDOG
COBJS  += watchdog.o watchdog_phy_v3.o watchdog_hal.o
endif

ifdef CONFIG_AUTODLOADER
COBJS   += chip_cfg.o
endif

ifdef CONFIG_SPX30G
PHYDIR = chip_x30g
$(shell mkdir -p $(obj)$(PHYDIR))
COBJS += $(PHYDIR)/chip_phy_x30g.o
COBJS += Tshark_boot_cp.o
else
COBJS += shark_boot_cp.o
endif

COBJS += misc.o
ifdef CONFIG_SPX15

PHYDIR  = chip_x15
$(shell mkdir -p $(obj)$(PHYDIR))
COBJS += $(PHYDIR)/low_power.o
endif


#ifneq ($(strip($(CONFIG_ADIE_SC2723)$(CONFIG_ADIE_SC2723S))),)
ifneq ($(strip $(CONFIG_ADIE_SC2723)$(CONFIG_ADIE_SC2723S)),)
COBJS += dcdc_cal_sc2723.o
else

ifdef CONFIG_ADIE_SC2713S
COBJS += dcdc_cal_sc2713s.o
ifdef CONFIG_ADIE_SC2713
COBJS += dcdc_cal.o
endif

else
ifdef CONFIG_SPX15
COBJS += dcdc_cal_x15.o
else
COBJS += dcdc_cal.o
endif
endif # CONFIG_ADIE_SC2713S
endif # CONFIG_ADIE_SC2723


SRCS	:= $(SOBJS:.o=.S) $(COBJS:.o=.c)
OBJS	:= $(addprefix $(obj),$(SOBJS) $(COBJS) )

all:	$(obj).depend $(LIB)

$(LIB):	$(OBJS)
	$(AR) $(ARFLAGS) $@ $(OBJS)

#########################################################################

# defines $(obj).depend target
include $(SRCTREE)/rules.mk

sinclude $(obj).depend

#########################################################################
