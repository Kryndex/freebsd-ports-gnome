# Created by: Aron Schlesinger <as@bsdgroup.de>
# $FreeBSD$

PORTNAME=	groupoffice
DISTVERSIONPREFIX=	com-
DISTVERSION=	6.2.75
CATEGORIES=	www
MASTER_SITES=	SF/group-office/${DISTVERSION:R}

MAINTAINER=	joneum@FreeBSD.org
COMMENT=	Modular web application framework for office

LICENSE=	AGPLv3
LICENSE_FILE=	${WRKSRC}/LICENSE.AGPL

RESTRICTED=	Free for personal use only

NO_ARCH=	yes
NO_BUILD=	yes
USES=		mysql shebangfix php:web python
USE_PHP=	mysqli session mbstring pcre xml
SHEBANG_FILES=	go/vendor/tcpdf/tools/tcpdf_addfont.php \
		groupoffice \
		groupofficecli.php \
		install/autoinstall.php \
		modules/postfixadmin/scripts/vacation.pl \
		modules/sync/z-push/install.sh \
		modules/sync/z-push/z-push-admin.php \
		modules/sync/z-push/z-push-top.php \
		vendor/fkooman/php-oauth-client/bin/php-oauth-client-create-tables \
		vendor/sabre/dav/bin/naturalselection \
		vendor/sabre/dav/bin/googlecode_upload.py \
		vendor/bin/../sabre/dav/bin/naturalselection \
		vendor/swiftmailer/swiftmailer/lib/swiftmailer_generate_mimes_config.php

WWWDIR?=	${PREFIX}/www/${PORTNAME}${DISTVERSION:R:R}

SUB_FILES=	pkg-message

OPTIONS_DEFINE=	IMAP GD CALENDAR WBXML
OPTIONS_DEFAULT=IMAP GD CALENDAR
CALENDAR_DESC=	Calendar holiday support
IMAP_DESC=	IMAP support
GD_DESC=	Gallery support (GD)
WBXML_DESC=	Synchronization Support

CALENDAR_USE=		PHP=calendar
GD_USE=			PHP=gd
IMAP_USE=		PHP=iconv imap
WBXML_LIB_DEPENDS=	libwbxml2.so:textproc/wbxml2

post-patch:
	@${REINPLACE_CMD} 's|%%LOCALBASE%%|${LOCALBASE}|g' \
		${WRKSRC}/go/base/Config.php
	@${REINPLACE_CMD} 's|/etc/groupoffice|${PREFIX}&|g' \
		${WRKSRC}/go/base/Config.php \
		${WRKSRC}/install/configFile.php \
		${WRKSRC}/modules/serverclient/HttpClient.php \
		${WRKSRC}/modules/serverclient/ServerclientModule.php \
		${WRKSRC}/modules/site/index.php

do-install:
	@${MKDIR} ${STAGEDIR}${WWWDIR}
	${CP} -R ${WRKSRC}/ ${STAGEDIR}${WWWDIR}
	@${TOUCH} ${STAGEDIR}${WWWDIR}/config.php.sample

.include <bsd.port.mk>
