# makefile for laslib
#
#COPTS    = -g -Wall
COPTS     = -O3 -Wall -Wno-deprecated -DNDEBUG -DUNORDERED -fno-strict-aliasing
COMPILER  = g++
AR  = ar rc
#BITS     = -64

INCLUDE = -I/usr/include/ -I../../LASzip/src -I../inc -I.

OBJ_LAS		= lasreader.o laswriter.o lasreader_las.o lasreader_bin.o lasreader_qfit.o lasreader_shp.o lasreader_asc.o lasreader_bil.o lasreader_dtm.o lasreader_txt.o lasreadermerged.o lasreaderbuffered.o lasreaderpipeon.o laswriter_las.o laswriter_bin.o laswriter_qfit.o laswriter_wrl.o laswriter_txt.o laswritercompatible.o laswaveform13reader.o laswaveform13writer.o lasutility.o lasfilter.o lastransform.o fopen_compressed.o

LAZ_DIR = ../../LASzip/src
OBJ_LAZ = laszip.o lasreadpoint.o lasreaditemcompressed_v1.o lasreaditemcompressed_v2.o laswritepoint.o  laswriteitemcompressed_v1.o laswriteitemcompressed_v2.o integercompressor.o arithmeticdecoder.o arithmeticencoder.o arithmeticmodel.o lasindex.o lasquadtree.o lasinterval.o 

TARGET_LAS_OBJS = ${patsubst %.o,${OBJDIR}/%.o,${OBJ_LAS}}
TARGET_LAZ_OBJS = ${patsubst %.o,${LAZ_DIR}/${OBJDIR}/%.o,${OBJ_LAZ}}

all: static shared

static:
	test -d static || mkdir static
	test -d ${LAZ_DIR}/static || mkdir ${LAZ_DIR}/static
	$(MAKE) libLASlib.a OBJDIR=static

shared:
	test -d shared || mkdir shared
	test -d ${LAZ_DIR}/shared || mkdir ${LAZ_DIR}/shared
	$(MAKE) libLASlib.so.${VERSION} OBJDIR=shared EXTRA_COPTS=-fPIC
   
libLASlib.a: ${TARGET_LAS_OBJS} ${TARGET_LAZ_OBJS}
	$(AR) $@ ${TARGET_LAS_OBJS} ${TARGET_LAZ_OBJS}
	cp -p $@ ../lib

libLASlib.so.${VERSION}: ${TARGET_LAS_OBJS} ${TARGET_LAZ_OBJS}
	${COMPILER} -shared -Wl,-soname,libLASlib.so.${VERSION} -o \
	libLASlib.so.${VERSION} ${TARGET_LAS_OBJS} ${TARGET_LAZ_OBJS}
	cp -p $@ ../lib
	ln -s libLASlib.so.${VERSION} ../lib/libLASlib.so

${TARGET_LAS_OBJS}: ${OBJDIR}/%.o: %.cpp
	${COMPILER} ${BITS} -c ${COPTS} ${EXTRA_COPTS} ${INCLUDE} $< -o $@

${TARGET_LAZ_OBJS}: ${LAZ_DIR}/${OBJDIR}/%.o: ${LAZ_DIR}/%.cpp
	${COMPILER} ${BITS} -c ${COPTS} ${EXTRA_COPTS} ${INCLUDE} $< -o $@

clean:
	rm -rf libLASlib.*
	if [ -d static ]; then rm -rf static; fi
	if [ -d shared ]; then rm -rf shared; fi
	if [ -d ${LAZ_DIR}/static ]; then rm -rf ${LAZ_DIR}/static; fi
	if [ -d ${LAZ_DIR}/shared ]; then rm -rf ${LAZ_DIR}/shared; fi

clobber: clean
	rm -rf libLASlib.*
	rm -rf ${OBJ_LAZ} ${OBJ_LAS}
	rm -rf *~

.PHONY: shared static clean clobber
