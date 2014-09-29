# Binaries will be generated with this name (.elf, .bin, .hex, etc)
PROJ_NAME=GPIO_EXTI




# Put your source files here (or *.c, etc)
SRCS_PATH=../../STM32Cube_FW_F4_V1.3.0_lite/Projects/STM32F429I-Discovery/Examples/GPIO/GPIO_EXTI/Src
SRCS_CC  = $(SRCS_PATH)/main.c 
SRCS_CC += $(SRCS_PATH)/stm32f4xx_it.c 
SRCS_CC += $(SRCS_PATH)/system_stm32f4xx.c

SRCS_HAL_PATH=../../STM32Cube_FW_F4_V1.3.0_lite/Drivers/STM32F4xx_HAL_Driver/Src
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_cortex.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_dma.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_flash.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_flash_ex.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_gpio.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_i2c.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_i2c_ex.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_pwr.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_pwr_ex.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_rcc.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_rcc_ex.c
SRCS_CC += $(SRCS_HAL_PATH)/stm32f4xx_hal_spi.c

SRCS_CC += ../../STM32Cube_FW_F4_V1.3.0_lite/Drivers/BSP/STM32F429I-Discovery/stm32f429i_discovery.c

# add startup file to build
SRCS_AS = $(SRCS_PATH)/../TrueSTUDIO/startup_stm32f429xx.s 

INC_DIRS += -I.
INC_DIRS += -I$(SRCS_PATH)/../Inc
INC_DIRS += -I$(SRCS_PATH)/../../../../../../Drivers/CMSIS/Include
INC_DIRS += -I$(SRCS_PATH)/../../../../../../Drivers/CMSIS/Device/ST/STM32F4xx/Include
INC_DIRS += -I$(SRCS_PATH)/../../../../../../Drivers/STM32F4xx_HAL_Driver/Inc
INC_DIRS += -I$(SRCS_PATH)/../../../../../../Drivers/BSP/STM32F429I-Discovery

# Put your stlink folder here so make burn will work.
STLINK=~/STM32/stlink.git

# Normally you shouldn't need to change anything below this line!
#######################################################################################

CC=arm-none-eabi-gcc
AS=arm-none-eabi-as
# LD=arm-none-eabi-ld: gcc used insteqd of ld -> a bit smarter (find libc.a)
LD=arm-none-eabi-gcc

OBJCOPY=arm-none-eabi-objcopy

CFLAGS  = -g -O2 -Wall
CFLAGS += -DSTM32F429xx -DUSE_HAL_DRIVER -DUSE_STM32F429I_DISCO
CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
CFLAGS += -mfloat-abi=hard -mfpu=fpv4-sp-d16
CFLAGS += $(INC_DIRS)

AFLAGS = -g -Wall

LDFLAGS  = $(CFLAGS) 
LDFLAGS += -T$(SRCS_PATH)/../TrueSTUDIO/STM32F429I-Discovery/STM32F429ZI_FLASH.ld
LDFLAGS += -specs=nosys.specs


OBJS_CC = $(SRCS_CC:.c=.o)
OBJS_AS = $(SRCS_AS:.s=.o)

.PHONY: proj

all: proj

proj: $(PROJ_NAME).elf

%.o: %.c
	@echo CC-Compiling [$<]
	@$(CC) -o $@ -c $< $(CFLAGS)
	
%.o: %.s
	@echo AS-Compiling [$<]
	@$(AS) -o $@ $< $(AFLAGS)

$(PROJ_NAME).elf: $(OBJS_CC) $(OBJS_AS)
	@echo Linking [$@]
	@$(LD) -o $@ $^ $(LDFLAGS)
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

clean-cc:
	rm -f $(OBJS_CC) $(PROJ_NAME).elf $(PROJ_NAME).hex $(PROJ_NAME).bin
	
clean: clean-cc
	rm -f $(OBJS_AS)

# Flash the STM32F4
burn: proj
	$(STLINK)/st-flash write $(PROJ_NAME).bin 0x8000000
