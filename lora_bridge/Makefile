APP_NAME = lora_bridge
### constant symbols
ARCH ?=
CROSS_COMPILE ?= arm-linux-gnueabihf-
CC := $(CROSS_COMPILE)gcc
AR := $(CROSS_COMPILE)ar

CFLAGS=-O2 -Wall -Wextra -std=c99 -Iinc -I.

## set liblgw path
LGW_PATH = ../lora_gateway/libloragw
LGW_INC = $(LGW_PATH)/inc/loragw_hal.h $(LGW_PATH)/inc/loragw_aux.h $(LGW_PATH)/inc/config.h

LIBS := -lloragw -lrt -lm -lpthread

## make all
all : $(APP_NAME)

## clean
clean:
	rm -f obj/*.o
	rm -f $(APP_NAME)
	$(MAKE) clean -C $(LGW_PATH)

### HAL library (do no force multiple library rebuild even with 'make -B')

$(LGW_PATH)/inc/config.h:
	@if test ! -f $@; then \
	$(MAKE) all -C $(LGW_PATH); \
	fi

$(LGW_PATH)/libloragw.a: $(LGW_INC)
	@if test ! -f $@; then \
	$(MAKE) all -C $(LGW_PATH); \
	fi

### Main program compilation and assembly

obj/parson.o: src/parson.c inc/parson.h
	$(CC) -c $(CFLAGS) $< -o $@

obj/$(APP_NAME).o: src/$(APP_NAME).c $(LGW_INC) inc/parson.h
	$(CC) -c $(CFLAGS) -I$(LGW_PATH)/inc $< -o $@

$(APP_NAME): obj/$(APP_NAME).o $(LGW_PATH)/libloragw.a obj/parson.o
	$(CC) -L$(LGW_PATH) $< obj/parson.o -o $@ $(LIBS)

### EOF

