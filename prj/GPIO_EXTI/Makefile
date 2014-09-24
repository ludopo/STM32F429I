# Binaries will be generated with this name (.elf, .bin, .hex, etc)
PROJ_NAME=GPIO_EXTI




# Put your source files here (or *.c, etc)
SRCS_PATH=../../STM32Cube_FW_F4_V1.3.0_lite/Projects/STM32F429I-Discovery/Examples/GPIO/GPIO_EXTI/Src
SRCS  = $(SRCS_PATH)/main.c 
SRCS += $(SRCS_PATH)/stm32f4xx_it.c 
SRCS += $(SRCS_PATH)/system_stm32f4xx.c

# add startup file to build
ASMS = $(SRCS_PATH)/../TrueSTUDIO/startup_stm32f429xx.s 

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
OBJCOPY=arm-none-eabi-objcopy

CFLAGS  = -g -O2 -Wall
CFLAGS += -DSTM32F429xx
CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
CFLAGS += -mfloat-abi=hard -mfpu=fpv4-sp-d16
CFLAGS += $(INC_DIRS)



LDFLAGS  = $(CFLAGS)
LDFLAGS += -T$(SRCS_PATH)/../TrueSTUDIO/STM32F429I-Discovery/STM32F429ZI_FLASH.ld


OBJS = $(SRCS:.c=.o)
OBJS_ASM = $(SRCS:.s=.o)

.PHONY: proj

all: proj

proj: $(PROJ_NAME).elf

%.o: %.c
	@echo Compiling [$<]
	@$(CC) -o $@ -c $< $(CFLAGS)
	
%.o: %.s
	@echo Compiling [$<]
	@$(CC) -o $@ $< $(CFLAGS)

$(PROJ_NAME).elf: $(OBJS) $(OBJS_ASM)
	@echo Linking[$@]
	@$(CC) -o $@ $^ $(LDFLAGS)
#	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
#	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

clean:
	rm -f *.o $(PROJ_NAME).elf $(PROJ_NAME).hex $(PROJ_NAME).bin

# Flash the STM32F4
burn: proj
	$(STLINK)/st-flash write $(PROJ_NAME).bin 0x8000000
