# Created by: Kevin Lo <kevlo@FreeBSD.org>
# $FreeBSD$

PORTNAME=	freefilesync
PORTVERSION=	8.3
PORTREVISION=	8
CATEGORIES=	sysutils
MASTER_SITES=	https://www.freefilesync.org/download/ \
		https://www.freefilesync.org/archive/
DISTNAME=	FreeFileSync_${PORTVERSION}_Source

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Backup software to synchronize files and folders

LICENSE=	GPLv3
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	${LOCALBASE}/include/zenxml/xml.h:textproc/zenxml
LIB_DEPENDS=	libboost_system.so:devel/boost-libs \
		libnotify.so:devel/libnotify

FETCH_ARGS=	--user-agent=firefox

WRKSRC=		${WRKDIR}/FreeFileSync/Source
DOS2UNIX_WRKSRC=${WRKDIR}
DOS2UNIX_GLOB=	*.cpp *.h

USES=		compiler:c++14-lang dos2unix gmake pkgconfig zip
USE_LDCONFIG=	yes
USE_WX=		3.0+

post-patch:
	@${REINPLACE_CMD} -e 's/wx-config/$${WX_CONFIG:T}/' \
		-e 's,g++,$${CXX},' ${WRKSRC}/Makefile
	@${REINPLACE_CMD} -e 's|<sys/vfs.h>|<sys/mount.h>|' \
		${WRKDIR}/zen/file_access.cpp
	@${REINPLACE_CMD} -e 's|WEXITSTATUS||' ${WRKDIR}/zen/shell_execute.h
	@${REINPLACE_CMD} -e 's| noexcept;|;|' ${WRKDIR}/zen/scope_guard.h
	@${REINPLACE_CMD} -e 's|CLOCK_MONOTONIC_RAW|CLOCK_MONOTONIC|' \
		${WRKDIR}/zen/tick_count.h

.include <bsd.port.mk>
