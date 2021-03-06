# Created by: Alejandro Pulver <alejandro@varnet.biz>
# $FreeBSD$

PORTNAME=	tuxmath
PORTVERSION=	2.0.3
PORTREVISION=	6
PORTEPOCH=	1
CATEGORIES=	games
MASTER_SITES=	DEBIAN_POOL \
		DEBIAN/pool/main/t/t4kcommon:t4k_common
DISTFILES=	${PORTNAME}_${PORTVERSION}.orig${EXTRACT_SUFX} \
		${T4K_DISTFILE}:t4k_common

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Educational arcade game starring Tux

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/doc/COPYING_GPL3

WRKSRC=		${WRKDIR}/${PORTNAME}_w_fonts-${PORTVERSION}

USE_GNOME=	libxml2
USE_SDL=	image mixer net pango sdl
USES=		gmake iconv localbase perl5 pkgconfig
GNU_CONFIGURE=	yes
CONFIGURE_ENV=	T4K_COMMON_CFLAGS="-I${T4K_WRKSRC}/src" \
		T4K_COMMON_LIBS="${T4K_WRKSRC}/src/.libs/libt4k_common.a" \
		ac_cv_lib_m_csin=yes

CPPFLAGS+=	$$(pkg-config --cflags SDL_Pango libpng) \
		-DHAVE_ALPHASORT -DHAVE_SCANDIR -DHAVE_FSYNC -DICONV_CONST=""
LDFLAGS+=	$$(pkg-config --libs SDL_Pango libpng) ${ICONV_LIB}

T4K_NAME=	t4k_common
T4K_VERSION=	0.1.1
T4K_DISTFILE=	${T4K_NAME:S/_//}_${T4K_VERSION}.orig${EXTRACT_SUFX}
T4K_WRKSRC=	${WRKDIR}/${T4K_NAME}-${T4K_VERSION}

OPTIONS_DEFINE=	NLS SVG DOCS
OPTIONS_DEFAULT=	SVG
OPTIONS_SUB=	yes

NLS_CONFIGURE_ENABLE=	nls
NLS_USES=	gettext
NLS_LDFLAGS=	-lintl

SVG_CONFIGURE_WITH=	rsvg
SVG_USE=	GNOME=librsvg2

post-extract:
	@${LN} -sf ${T4K_WRKSRC} ${WRKDIR}/${T4K_NAME}

post-patch:
	@${REINPLACE_CMD} -e \
		'/doc[[:space:]]/d' ${WRKSRC}/Makefile.in
	@${REINPLACE_CMD} -e \
		's|-Wall -g |-Wall | ; \
		 s|-DDEBUG ||' ${WRKSRC}/src/Makefile.in

pre-configure:
	@(cd ${T4K_WRKSRC} && ${SETENV} ${CONFIGURE_ENV} ./configure \
		--disable-shared ${CONFIGURE_ARGS} CC="${CC}" CXX="${CXX}" \
		CFLAGS="${CFLAGS}" CXXFLAGS="${CXXFLAGS}" \
		CPPFLAGS="${CPPFLAGS}" LDFLAGS="${LDFLAGS}")

pre-build:
	@(cd ${T4K_WRKSRC} && ${DO_MAKE_BUILD} ${ALL_TARGET})

post-install:
	${LN} -s status ${STAGEDIR}${DATADIR}/images/menu

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
.for file in OFL README README_DATA_LICENSES TODO changelog
	${INSTALL_DATA} ${WRKSRC}/doc/${file} ${STAGEDIR}${DOCSDIR}
.endfor

.include <bsd.port.mk>
