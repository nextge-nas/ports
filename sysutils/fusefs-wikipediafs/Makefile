# Created by: Andrew Pantyukhin <infofarmer@FreeBSD.org>
# $FreeBSD$

PORTNAME=	wikipediafs
PORTVERSION=	0.4
PORTREVISION=	1
CATEGORIES=	sysutils
MASTER_SITES=	SF
PKGNAMEPREFIX=	fusefs-

MAINTAINER=	bofh@FreeBSD.org
COMMENT=	Mount mediawikis

LICENSE=	GPLv2

DEPRECATED=	Uses deprecated version of python
EXPIRATION_DATE=	2020-09-15

RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}fusefs>=0.2:devel/py-fusefs@${PY_FLAVOR}

USES=		fuse python:2.7
USE_PYTHON=	autoplist distutils
PORTDOCS=	AUTHORS ChangeLog HACKING NEWS README TODO
NO_ARCH=	yes

OPTIONS_DEFINE=	DOCS

PLIST_FILES=	man/man1/mount.${PORTNAME}.1.gz

post-extract:
	@${GUNZIP_CMD} ${WRKSRC}/doc/*gz

post-install:
	@cd ${WRKSRC}/doc && ${INSTALL_MAN} mount.${PORTNAME}.1 ${STAGEDIR}${PREFIX}/man/man1/

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	cd ${WRKSRC} && ${INSTALL_DATA} ${PORTDOCS} ${STAGEDIR}${DOCSDIR}/

.include <bsd.port.mk>
