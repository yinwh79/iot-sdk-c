CFLAGS = -g -O2 -std=gnu99
DFLAGS = -DUSE_MBED_TLS

# set these variables to mbedtls lib/include dir path on your system
MBED_LIB_DIR = .
MBED_INC_DIR = .

LDFLAGS =  -L$(MBED_LIB_DIR) -lmbedtls -lmbedcrypto -lmbedx509
LDFLAGS +=  -lm -lpthread -lrt -ldl 
TARGET := iot_smarthome_sample_test

# modify to your cross-compiler name if your are cross-compiling
CROSS_COMPILE = mipsel-openwrt-linux-
CC = gcc

BUILD_BASE = ./build
OBJ_BASE = $(BUILD_BASE)/objs
EDGE_SDK_DIR=../../../..


V ?= $(VERBOSE)
ifeq ("$(V)","1")
Q :=
vecho := @true
else
Q := @
vecho := @echo
endif

# modify mbedtls include dir path on your system
INC = -I$(MBED_INC_DIR) \
-I$(EDGE_SDK_DIR)/c-utility/inc  \
-I$(EDGE_SDK_DIR)/iothub_client/inc \
-I$(EDGE_SDK_DIR)/umqtt/inc  \
-I$(EDGE_SDK_DIR)/umqtt/inc/azure_umqtt_c 	\
-I$(EDGE_SDK_DIR)/c-utility/pal/generic \
-I$(EDGE_SDK_DIR)/certs \
-I$(EDGE_SDK_DIR)/c-utility/pal/inc \
-I$(EDGE_SDK_DIR)/parson \
-I$(EDGE_SDK_DIR)/serializer/inc \
-I$(EDGE_SDK_DIR)/iothub_client/samples/iot_smarthome_client_sample


SOURCES = \
$(EDGE_SDK_DIR)/c-utility/src/xlogging.c \
$(EDGE_SDK_DIR)/c-utility/src/buffer.c \
$(EDGE_SDK_DIR)/c-utility/src/consolelogger.c \
$(EDGE_SDK_DIR)/c-utility/src/crt_abstractions.c \
$(EDGE_SDK_DIR)/c-utility/src/doublylinkedlist.c \
$(EDGE_SDK_DIR)/c-utility/src/utf8_checker.c \
$(EDGE_SDK_DIR)/c-utility/src/httpheaders.c \
$(EDGE_SDK_DIR)/c-utility/src/map.c \
$(EDGE_SDK_DIR)/c-utility/src/optionhandler.c \
$(EDGE_SDK_DIR)/c-utility/src/strings.c \
$(EDGE_SDK_DIR)/c-utility/src/vector.c \
$(EDGE_SDK_DIR)/c-utility/src/xio.c \
$(EDGE_SDK_DIR)/c-utility/src/singlylinkedlist.c \
$(EDGE_SDK_DIR)/serializer/src/agenttypesystem.o \
$(EDGE_SDK_DIR)/serializer/src/codefirst.o \
$(EDGE_SDK_DIR)/serializer/src/commanddecoder.o \
$(EDGE_SDK_DIR)/serializer/src/datamarshaller.o \
$(EDGE_SDK_DIR)/serializer/src/datapublisher.o \
$(EDGE_SDK_DIR)/serializer/src/dataserializer.o \
$(EDGE_SDK_DIR)/serializer/src/iotdevice.o \
$(EDGE_SDK_DIR)/serializer/src/jsondecoder.o \
$(EDGE_SDK_DIR)/serializer/src/jsonencoder.o \
$(EDGE_SDK_DIR)/serializer/src/methodreturn.o \
$(EDGE_SDK_DIR)/serializer/src/multitree.o \
$(EDGE_SDK_DIR)/serializer/src/schema.o \
$(EDGE_SDK_DIR)/serializer/src/schemalib.o \
$(EDGE_SDK_DIR)/serializer/src/schemaserializer.o \
$(EDGE_SDK_DIR)/iothub_client/src/http_signer.c \
$(EDGE_SDK_DIR)/c-utility/src/hmac.c \
$(EDGE_SDK_DIR)/c-utility/src/hmacsha256.c \
$(EDGE_SDK_DIR)/c-utility/src/urlencode.c \
$(EDGE_SDK_DIR)/c-utility/src/base64.c \
$(EDGE_SDK_DIR)/c-utility/src/usha.c \
$(EDGE_SDK_DIR)/c-utility/src/sha1.c \
$(EDGE_SDK_DIR)/c-utility/src/sha224.c \
$(EDGE_SDK_DIR)/c-utility/src/sha384-512.c \
$(EDGE_SDK_DIR)/c-utility/src/sastoken.c \
$(EDGE_SDK_DIR)/c-utility/src/httpapiex.c \
$(EDGE_SDK_DIR)/c-utility/src/httpapiexsas.c \
$(EDGE_SDK_DIR)/certs/certs.c \
$(EDGE_SDK_DIR)/c-utility/adapters/httpapi_compact.c \
\
\
$(EDGE_SDK_DIR)/iothub_client/src/bos.c \
$(EDGE_SDK_DIR)/iothub_client/src/rsa_signer/rsa_signer_mbedtls.c \
$(EDGE_SDK_DIR)/iothub_client/src/iot_smarthome_client.c \
$(EDGE_SDK_DIR)/iothub_client/src/iothub_client_persistence.c \
$(EDGE_SDK_DIR)/iothub_client/src/iothub_mqtt_client.c \
\
\
$(EDGE_SDK_DIR)/umqtt/src/mqtt_client.c \
$(EDGE_SDK_DIR)/umqtt/src/mqtt_codec.c \
$(EDGE_SDK_DIR)/umqtt/src/mqtt_message.c \
\
\
$(EDGE_SDK_DIR)/c-utility/adapters/agenttime.c \
$(EDGE_SDK_DIR)/c-utility/adapters/threadapi_pthreads.c \
$(EDGE_SDK_DIR)/c-utility/adapters/tickcounter_linux.c \
$(EDGE_SDK_DIR)/c-utility/adapters/socketio_berkeley.c \
$(EDGE_SDK_DIR)/c-utility/adapters/lock_pthreads.c \
$(EDGE_SDK_DIR)/c-utility/adapters/linux_time.c \
$(EDGE_SDK_DIR)/c-utility/adapters/platform_linux.c \
$(EDGE_SDK_DIR)/c-utility/src/tlsio_mbedtls.c \
$(EDGE_SDK_DIR)/c-utility/pal/tlsio_options.c \
$(EDGE_SDK_DIR)/parson/parson.c \
\
$(EDGE_SDK_DIR)/iothub_client/samples/iot_smarthome_client_sample/iot_smarthome_client_sample.c \
./main.c


vpath %.c $(EDGE_SDK_DIR)/c-utility/src \
$(EDGE_SDK_DIR)/iothub_client/src \
$(EDGE_SDK_DIR)/umqtt/src \
$(EDGE_SDK_DIR)/c-utility/adapters \
$(EDGE_SDK_DIR)/c-utility/pal \
$(EDGE_SDK_DIR)/parson \
$(EDGE_SDK_DIR)/certs \
$(EDGE_SDK_DIR)/iothub_client/samples/iot_smarthome_client_sample \
$(EDGE_SDK_DIR)/serializer/src \
$(EDGE_SDK_DIR)/iothub_client/src/rsa_signer \

OBJS = $(notdir $(patsubst %.c,%.o,$(SOURCES)))

%.o:%.c | out
	$(vecho) "CC $<"
	$(Q) $(CROSS_COMPILE)$(CC) $(DFLAGS) $(CFLAGS) $(INC) -o $(addprefix $(OBJ_BASE)/,$@) -c $<

$(BUILD_BASE)/$(TARGET):$(OBJS)
	$(vecho) "TARGET $@"
	$(Q) $(CROSS_COMPILE)$(CC)  -o $(BUILD_BASE)/$(TARGET) $(addprefix $(OBJ_BASE)/,$(OBJS)) $(LDFLAGS)

all: $(BUILD_BASE)/$(TARGET)

.PHONY : clean
clean:
	$(Q) rm -rf $(OBJ_BASE)
	$(Q) rm -f $(BUILD_BASE)/$(TARGET)

out:
	$(Q) mkdir -p $(BUILD_BASE)
	$(Q) mkdir -p $(OBJ_BASE)
