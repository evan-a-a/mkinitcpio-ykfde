# commands
CC	:= gcc
INSTALL	:= install
RM	:= rm

# paths
LIBDIR  ?= /usr/lib/ykfde

# flags
CFLAGS		+= -std=gnu11 -O2 -fPIC -Wall -Werror
CFLAGS_YUBIKEY	+= -liniparser -lkeyutils -lykpers-1 -lyubikey
CFLAGS_SYSTEMD	+= $(shell pkg-config --cflags --libs libsystemd 2>/dev/null)
LDFLAGS		+= -Wl,-z,now -Wl,-z,relro -pie

all: ykfde-worker ykfde ykfde-cpio

ykfde-worker: ykfde-worker.c ../config.h
	$(CC) $(CFLAGS) $(CFLAGS_YUBIKEY) $(CFLAGS_SYSTEMD) $(LDFLAGS) -o $@ $<

ykfde: ykfde.c ../config.h ../version.h
	$(CC) $(CFLAGS) $(CFLAGS_YUBIKEY) $(CFLAGS_SYSTEMD) -lcryptsetup $(LDFLAGS) -o $@ $<

ykfde-cpio: ykfde-cpio.c ../config.h ../version.h
	$(CC) $(CFLAGS) -larchive $(LDFLAGS) -o $@ $<

install: ykfde-worker ykfde ykfde-cpio
	$(INSTALL) -D -m0755 ykfde-worker $(DESTDIR)$(LIBDIR)/ykfde-worker
	$(INSTALL) -D -m0755 ykfde $(DESTDIR)/usr/bin/ykfde
	$(INSTALL) -D -m0755 ykfde-cpio $(DESTDIR)/usr/bin/ykfde-cpio

clean:
	$(RM) -f ykfde-worker ykfde ykfde-cpio
