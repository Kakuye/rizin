include ../../../config-user.mk
include ../../../librz/config.mk
include ../../../mk/platform.mk
include ../../../mk/${COMPILER}.mk

CFLAGS+=${PIC_CFLAGS}

ifneq (,$(findstring cygwin,${OSTYPE}))
CFLAGS+=-D__CYGWIN__=1
EXT_SO=dll
SOVER=${EXT_SO}
LDFLAGS+=-shared
LDFLAGS_SHARED=-shared
else
ifneq (,$(findstring mingw32,${OSTYPE}))
CFLAGS+=-DMINGW32=1
EXT_SO=dll
SOVER=${EXT_SO}
LDFLAGS+=-shared
LDFLAGS_SHARED?=-shared
else
LDFLAGS_SHARED=-shared
endif
endif
ifeq (${OSTYPE},windows)
EXT_SO=dll
SOVER=${EXT_SO}
LDFLAGS+=-shared
LDFLAGS_SHARED=-shared
else
ifeq (${OSTYPE},darwin)
EXT_SO=dylib
SOVER=${EXT_SO}
LDFLAGS+=-dynamic
LDFLAGS_SHARED=-dynamiclib
# ifeq (${ARCH},i386)
   #CC+=-arch i386
#   CC+=-arch x86_64
# endif
else
SOVERSION=0
EXT_SO=so
SOVER=${EXT_SO}
LDFLAGS_SHARED+=-Wl,-soname,librz_z.${EXT_SO}
endif
endif

LDFLAGS_SHARED+=${PIC_CFLAGS}

OBJS = adler32.o crc32.o deflate.o infback.o \
	inffast.o inflate.o inftrees.o trees.o \
	zutil.o compress.o uncompr.o gzclose.o \
	gzlib.o gzread.o gzwrite.o

#OFILES= librz_z.$(EXT_AR) $(OBJS)
OFILES= $(OBJS)

CFLAGS+=-Wall
CFLAGS+=-I../../librz/include -I../include/
#LDFLAGS+=-L../../../librz/util
LIBNAME=librz_z.${SOVER}
LIBAR=librz_z.$(EXT_AR)

all:: ${LIBAR}
ifeq ($(WITHPIC),1)
all:: ${LIBNAME}
endif

${LIBNAME}: ${OBJS}
ifneq ($(SILENT),)
	@echo LIB ${LIBNAME}
endif
	${CC} ${CFLAGS} ${LDFLAGS} $(LDFLAGS_SHARED) -o $@ ${OBJS}

librz_z.$(EXT_AR): ${OFILES}
	${CC_AR} ${OFILES}
	${RANLIB} librz_z.$(EXT_AR)

clean:
	rm -f ${LIBNAME} *.o *.$(EXT_AR) *.d *.dylib *.so
