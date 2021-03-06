#
# sensor lib Makefile
#

ifeq ($(PARAM_FILE), )
	PARAM_FILE:=../../../../Makefile.param
	include $(PARAM_FILE)
endif

ISP_PATH := $(SDK_PATH)/mpp/component/isp
EXT_PATH := $(SDK_PATH)/mpp/$(EXTDRV)
3A_PATH  := $(ISP_PATH)/3a
LIBPATH = $(ISP_PATH)/lib
OBJPATH = ./obj

ARFLAGS = rcv
ARFLAGS_SO = $(LIBS_LD_CFLAGS)
ARFLAGS_SO += -shared -fPIC -o

CFLAGS  = -Wall
CFLAGS  += -O2 -fPIC
CFLAGS  += $(LIBS_CFLAGS)

ifeq ($(HIGDB),HI_GDB)
CFLAGS += -g
endif
ifeq ($(CONFIG_JPEGEDCF), y)
     CFLAGS += -D ENABLE_JPEGEDCF 
endif
BUS_DIR := $(EXT_PATH)/ssp-sony/

ISP_INC := $(ISP_PATH)/include
3A_INC := $(3A_PATH)/include
INC := -I$(BUS_DIR) -I$(REL_INC) -I$(ISP_INC) -I$(3A_INC)

COMPILE = $(CC) $(CFLAGS) $(DFLAGS) -lm

$(OBJPATH)/%.o: ./%.c
	@[ -e $(LIBPATH) ] || mkdir $(LIBPATH)
	@[ -e $(OBJPATH) ] || mkdir $(OBJPATH)
	@$(COMPILE) -o $@ -c $< $(INC)

SRCS = $(wildcard ./*.c)
OBJS = $(SRCS:%.c=%.o)
OBJS := $(OBJS:./%=obj/%)

TARGETLIB := $(LIBPATH)/libsns_imx222.a
TARGETLIB_SO := $(LIBPATH)/libsns_imx222.so

all:$(TARGETLIB)
$(TARGETLIB):$(OBJS)
	@($(AR) $(ARFLAGS) $(TARGETLIB) $(OBJS))
	@($(CC) $(ARFLAGS_SO) $(TARGETLIB_SO) $(OBJS))

clean:
	@$(RM) -rf $(TARGETLIB) $(OBJS)
	@$(RM) -rf $(LIBPATH) $(OBJPATH)
