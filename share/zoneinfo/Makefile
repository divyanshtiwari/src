#	$OpenBSD: Makefile,v 1.15 2020/10/07 22:33:31 millert Exp $
#	$NetBSD: Makefile,v 1.14 1995/04/22 12:10:17 cgd Exp $

# Change the line below for your time zone (after finding the zone you want in
# the time zone files, or adding it to a time zone file).
# Alternately, if you discover you've got the wrong time zone, you can just
#	zic -l rightzone

# This line has been moved to /usr/src/etc/Makefile
LOCALTIME=	US/Pacific

# If you want something other than Eastern United States time as a template
# for handling POSIX-style time zone environment variables,
# change the line below (after finding the zone you want in the
# time zone files, or adding it to a time zone file).
# Alternately, if you discover you've got the wrong time zone, you can just
#	zic -p rightzone

POSIXRULES=	US/Pacific

# Use an absolute path name for TZDIR unless you're just testing the software.

TZDIR=	${DESTDIR}/usr/share/zoneinfo

# If you always want time values interpreted as "seconds since the epoch
# (not counting leap seconds)", use
# 	REDO=		posix_only
# below.  If you always want right time values interpreted as "seconds since
# the epoch" (counting leap seconds)", use
#	REDO=		right_only
# below.  If you want both sets of data available, with leap seconds not
# counted normally, use
#	REDO=		posix_right
# below.  If you want both sets of data available, with leap seconds counted
# normally, use
#	REDO=		right_posix
# below.

REDO=		posix_only

TDATA=		africa antarctica asia australasia \
		europe northamerica southamerica etcetera factory \
		backward
TABDATA=	iso3166.tab zone.tab zone1970.tab
DATA=		$(TDATA) $(TABDATA) leapseconds
USNO=		usno1988 usno1989 usno1989a usno1995 usno1997

ZIC=		zic

all:

posix_only: ${TDATA}
	(cd ${.CURDIR}/datfiles; \
	    ${ZIC} -d ${TZDIR} -L /dev/null ${TDATA})

right_only: leapseconds ${TDATA}
	(cd ${.CURDIR}/datfiles; \
	    ${ZIC} -d ${TZDIR} -L leapseconds ${TDATA})

other_two: leapseconds ${TDATA}
	(cd ${.CURDIR}/datfiles; \
	    ${ZIC} -d ${TZDIR}/posix -L /dev/null ${TDATA})
	(cd ${.CURDIR}/datfiles; \
	    ${ZIC} -d ${TZDIR}/right -L leapseconds ${TDATA})

posix_right: posix_only other_two

right_posix: right_only other_two

realinstall: ${DATA} ${REDO}
	(cd ${.CURDIR}/datfiles; \
	    ${ZIC} -d ${TZDIR} -p ${POSIXRULES})
	chown -R ${BINOWN}:${BINGRP} ${TZDIR}
	find ${TZDIR} \
		-type f -exec chmod a=r {} + -o \
		-type d -exec chmod a=rx,u+w {} +
	${INSTALL} -c -o root -g bin -m 644 ${.CURDIR}/datfiles/iso3166.tab \
	    ${DESTDIR}/usr/share/misc
	${INSTALL} -c -o root -g bin -m 644 ${.CURDIR}/datfiles/zone.tab \
	    ${DESTDIR}/usr/share/zoneinfo
	${INSTALL} -c -o root -g bin -m 644 ${.CURDIR}/datfiles/zone1970.tab \
	    ${DESTDIR}/usr/share/zoneinfo


.PATH:	${.CURDIR}/datfiles
.include <bsd.prog.mk>
