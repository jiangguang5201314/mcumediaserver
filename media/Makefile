###########################################
# Fichero de configuracion
###########################################
include ../config.mk

#OPTS+= -fPIC -DPIC -DNDEBUG

#DEBUG
ifeq ($(DEBUG),yes)
	TAG=debug
	OPTS+= -g -O0 -DMCUDEBUG
else
	OPTS+= -O3
	TAG=release
endif

#LOG
ifeq ($(LOG),yes)
	OPTS+= -DLOG_
endif

############################################
#Directorios
############################################
BUILD = $(SRCDIR)/media/build/$(TAG)
BIN   = $(SRCDIR)/bin//$(TAG)

############################################
#Objetos
############################################
G711DIR=g711
G711OBJ=g711.o pcmucodec.o pcmacodec.o

H263DIR=h263
H263OBJ=h263.o h263codec.o mpeg4codec.o h263-1996codec.o

JSR309DIR=jsr309
JSR309OBJ=RTPEndpoint.o Player.o Endpoint.o MediaSession.o JSR309Manager.o RTPMultiplexer.o xmlrpcjsr309.o AudioDecoderWorker.o AudioEncoderWorker.o AudioMixerResource.o VideoDecoderWorker.o VideoEncoderWorker.o VideoMixerResource.o RTPMultiplexerSmoother.o Recorder.o

ifeq ($(FLV1PARSER),yes)
	FLV1DIR=flv1
	FLV1OBJ=flv1codec.o flv1Parser.o
	OPTS+= -DFLV1PARSER
else
	FLV1DIR=flv1
	FLV1OBJ=flv1codec.o
endif


H264DIR=h264
H264OBJ=h264encoder.o h264decoder.o h264depacketizer.o

GSMDIR=gsm
GSMOBJ=gsmcodec.o

SPEEXDIR=speex
SPEEXOBJ=speexcodec.o

NELLYDIR=nelly
NELLYOBJ=NellyCodec.o

OBJS=codecs.o video.o mcu.o multiconf.o rtpparticipant.o rtmpparticipant.o videomixer.o audiomixer.o xmlrpcserver.o xmlhandler.o xmlstreaminghandler.o statushandler.o xmlrpcmcu.o  tools.o rtpsession.o audiostream.o groupaudiostream.o videostream.o groupvideostream.o pipeaudioinput.o pipeaudiooutput.o pipevideoinput.o pipevideooutput.o framescaler.o mosaic.o partedmosaic.o asymmetricmosaic.o pipmosaic.o logo.o overlay.o amf.o rtmpmessage.o rtmpchunk.o rtmpstream.o rtmpconnection.o  rtmpserver.o broadcaster.o broadcastsession.o rtmpflvstream.o flvrecorder.o FLVEncoder.o xmlrpcbroadcaster.o mediagateway.o mediabridgesession.o xmlrpcmediagateway.o textmixer.o textmixerworker.o textstream.o pipetextinput.o pipetextoutput.o mp4player.o mp4streamer.o audioencoder.o textencoder.o mp4recorder.o rtmpmp4stream.o avcdescriptor.o RTPSmoother.o rtp.o

OBJS+= $(G711OBJ) $(H263OBJ) $(GSMOBJ)  $(H264OBJ) ${FLV1OBJ} $(SPEEXOBJ) $(NELLYOBJ) $(JSR309OBJ)
TARGETS=mcu rtmptest

ifeq ($(FLASHSTREAMER),yes)
	GNASHINCLUDE = -I$(GNASHBASE) -I$(GNASHBASE)/server -I$(GNASHBASE)/libbase -I$(GNASHBASE)/libgeometry -I$(GNASHBASE)/server/parser -I$(GNASHBASE)/server/vm -I$(GNASHBASE)/backend -I$(GNASHBASE)/libmedia -DFLASHSTREAMER 
	GNASHLD =  -lgnashserver -lagg  -L$(GNASHLIBS) 
	OBJS+= flash.o xmlrpcflash.o 
	OBJSFS   = flashstreamer.o FlashPlayer.o FlashSoundHandler.o $(OBJS)
	OBJSFSCLIENT = xmlrpcclient.o xmlrpcflashclient.o
	TARGETS += flashstreamer flashclient testflash
endif

OBJSMCU = $(OBJS) main.o
OBJSMCUCLIENT = xmlrpcclient.o xmlrpcmcuclient.o
OBJSRTMPTEST = $(OBJS) rtmptest.o

BUILDOBJSMCU = $(addprefix $(BUILD)/,$(OBJSMCU))
BUILDOBJSMCUCLIENT= $(addprefix $(BUILD)/,$(OBJSMCUCLIENT))
BUILDOBJSRTMPTEST= $(addprefix $(BUILD)/,$(OBJSRTMPTEST))
BUILDOBJSFS= $(addprefix $(BUILD)/,$(OBJSFS)) 
BUILDOBJSFSCLIENT= $(addprefix $(BUILD)/,$(OBJSFSCLIENT))

###################################
#Compilacion condicional
###################################
VPATH  =  %.o $(BUILD)/
VPATH +=  %.c $(SRCDIR)/media/src/
VPATH +=  %.cpp $(SRCDIR)/media/src/
VPATH +=  %.cpp $(SRCDIR)/media/src/$(G711DIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(GSMDIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(H263DIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(H264DIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(FLV1DIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(SPEEXDIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(NELLYDIR)
VPATH +=  %.cpp $(SRCDIR)/media/src/$(JSR309DIR)


INCLUDE+= -I$(SRCDIR)/media/include/ $(GNASHINCLUDE) -I/usr/local/src/live/UsageEnvironment/include/ -I/usr/local/src/live/BasicUsageEnvironment/include/ -I/usr/local/src/live/groupsock/include/ -I/usr/local/src/live/liveMedia/include/
LDFLAGS+= -L/usr/local/lib -lavcodec -lgsm -lpthread -lswscale -lavformat -lavutil -lx264 -lssl -lmp4v2 -lspeex -lspeexdsp -lcrypto
LDXMLFLAGS+= -lxmlrpc -lxmlrpc_xmlparse -lxmlrpc_xmltok -lxmlrpc_abyss -lxmlrpc_server -lxmlrpc_util
#LDLIVE555FLAGS+= -L/usr/local/src/live/liveMedia -lliveMedia -L/usr/local/src/live/BasicUsageEnvironment/ -lBasicUsageEnvironment -L/usr/local/src/live/UsageEnvironment/ -lUsageEnvironment -L/usr/local/src/live/groupsock/ -lgroupsock
LDFLAGS+= $(LDXMLFLAGS)
#LDFLAGS+= $(LDLIVE555FLAGS)

#For abyss
OPTS 	+= -D_UNIX -D__STDC_CONSTANT_MACROS -fPIC

CFLAGS  += $(INCLUDE) $(OPTS)
CXXFLAGS+= $(INCLUDE) $(OPTS)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $(BUILD)/$@

%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $(BUILD)/$@


############################################
#Targets
############################################
all: mkdirs $(TARGETS) 

mkdirs:  
	mkdir -p $(BUILD)
	mkdir -p $(BIN)
	cp $(SRCDIR)/media/logo.png $(BIN)
clean:
	rm -f $(BUILDOBJSMCU)
	rm -f $(BUILDOBJSFS)
	rm -f "$(BIN)/mcu"
	rm -f "$(BIN)/flashstreamer"

install:
	mkdir -p  $(TARGET)/lib
	mkdir -p  $(TARGET)/include/mcu
	

############################################
#MCU
############################################

mcu: $(OBJSMCU)
	$(CXX) -o $(BIN)/$@ $(BUILDOBJSMCU) $(LDFLAGS)

rtmptest: $(OBJSRTMPTEST)
	$(CXX) -o $(BIN)/$@ $(BUILDOBJSRTMPTEST) $(LDFLAGS)

flashstreamer: $(OBJSFS) $(OBJS)
	g++ -o $(BIN)/$@ $(BUILDOBJSFS) $(BUILDOBJS) $(GNASHBASE)/backend/.libs/libgnashagg.a /usr/lib/libagg.a $(LDFLAGS) $(GNASHLD)

