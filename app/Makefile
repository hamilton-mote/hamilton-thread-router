APPLICATION = openthread

# If no BOARD is found in the environment, use this default:
BOARD = hamilton
BOARD_WHITELIST := hamilton

# This has to be the absolute path to the RIOT base directory:
RIOTBASE = ../RIOT-OS

# This is to set the proper programmer for board
PROGRAMMER ?= jlink

# Comment this out to disable code in RIOT that does safety checking
# which is not needed in a production environment but helps in the
# development process:
#CFLAGS += -DDEVELHELP -Wall

# Change this to 0 show compiler invocation lines by default:
QUIET ?= 1

USEPKG += openthread
OPENTHREAD_TYPE ?= ftd
ifeq ($(OPENTHREAD_TYPE),mtd)
    # MTD: A Minimal Thread Device does not have router functionality
    # compiled in. As a result, it is not necessary to configure the
    # routerrole on an MTD. At the same time, an MTD may or may not be sleepy.
    USEMODULE += openthread-mtd
    USEMODULE += openthread-cli-mtd
    BOARD ?= hamilton
else
    # ftd: A Full Thread Device has router functionality compiled in
    USEMODULE += openthread-ftd
    USEMODULE += openthread-cli-ftd
    BOARD ?= samr21-xpro
endif

#Define PANID, CHANNEL and UART_BAUDRATE used by default
OPENTHREAD_PANID ?= 0xbeef
OPENTHREAD_CHANNEL ?= 26
OPENTHREAD_UART_BAUDRATE ?= 115200

CFLAGS += -DOPENTHREAD_PANID=${OPENTHREAD_PANID}
CFLAGS += -DOPENTHREAD_CHANNEL=${OPENTHREAD_CHANNEL}
CFLAGS += -DOPENTHREAD_UART_BAUDRATE=${OPENTHREAD_UART_BAUDRATE}
CFLAGS += -DOPENTHREAD_SOURCE=${HOSTNAME}

ifneq (,$(filter hamilton samr21-xpro,$(BOARD)))
  DRIVER := at86rf233
  CFLAGS    += -DAUTO_CSMA_EN=1
endif
ifneq (,$(filter iotlab-m3 fox iotlab-a8-m3,$(BOARD)))
  DRIVER := at86rf231
endif

ifneq (,$(filter at86rf2%,$(DRIVER)))
  FEATURES_REQUIRED += periph_spi periph_gpio
endif

USEMODULE += $(DRIVER)

# CPU clock speed
ifneq (,$(filter hamilton,$(BOARD)))
  CFLAGS    += -DCLOCK_USE_OSCULP32_DFLL=1
  CFLAGS    += -DCLOCK_USE_ADAPTIVE=0
  CFLAGS    += -DSENSOR_COMPUTOFFLOAD=1
  CFLAGS    += -DDMAC_ENABLE=0
  # Sensors
  USEMODULE += saul_reg        # SAUL: sensor/actuator abstraction layer
  USEMODULE += auto_init_saul
  USEMODULE += tmp006          # ambient temperature
  USEMODULE += hdc1000         # humidity and temperature
  USEMODULE += fxos8700        # acceleration and magnetic field
  USEMODULE += apds9007        # illumination
  USEMODULE += ekmb            # pir-based occupancy
  USEMODULE += pulse_counter   # simple button
endif

USEMODULE += xtimer
USEMODULE += random
USEMODULE += ps

USEMODULE += rtt_stdio
CFLAGS    += -DRTT_STDIO_DISABLE_STDIN
#CFLAGS    += -DRTT_STDIO_ENABLE_BLOCKING_STDOUT
#CFLAGS    += -DSTDIO_POLL_INTERVAL=5000U
CFLAGS    += -DTHREAD_STACKSIZE_DEFAULT=2048

#required for C++ compiling
CXXEXFLAGS += -fno-rtti
USEMODULE += cpp11-compat

include $(RIOTBASE)/Makefile.include
