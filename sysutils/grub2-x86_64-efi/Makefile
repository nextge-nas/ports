# Created by: sem@FreeBSD.org
# $FreeBSD$

PORTNAME=	grub2-x86_64-efi
PORTVERSION=	2.05
PORTREVISION=	1
CATEGORIES=	sysutils
DISTNAME=	grub-${PORTVERSION}

USE_GITHUB=	yes
GH_ACCOUNT=	rhboot
GH_PROJECT=	grub2
GH_TAGNAME=	e7b8856f8be3292afdb38d2e8c70ad8d62a61e10

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Multiboot boot loader

LICENSE=	GPLv3

BROKEN_i386=	uses amd64 assembly
DEPRECATED=	Unmaintained (more than five years), not updated (one-and-a-half years), does not build with modern compilers	# PR232961
EXPIRATION_DATE=2019-02-04

ONLY_FOR_ARCHS=	amd64 i386

BUILD_DEPENDS=	git:devel/git \
		${LOCALBASE}/bin/flex:textproc/flex \
		gsed:textproc/gsed \
		help2man:misc/help2man
RUN_DEPENDS=	gsed:textproc/gsed

USES=		autoreconf bison cpe gettext gmake tar:xz
USE_GCC=	7	# Fails to build with GCC 8, cf. PR 232961, or clang

GNU_CONFIGURE=	yes
INFO=		grub grub-dev
MAKE_JOBS_UNSAFE=	yes
SSP_UNSAFE=	yes

CPE_PRODUCT=	grub
CPE_VENDOR=	gnu

CONFIGURE_ARGS=	--with-platform=efi --target=x86_64 --disable-werror --localedir=${PREFIX}/share/locale 
CONFIGURE_ENV=	CPP="${CC} -E" \
		LEX=${LOCALBASE}/bin/flex

OPTIONS_DEFINE=	MKFONT FUSE
OPTIONS_SUB=	yes
MKFONT_DESC=	Build grub-mkfont (require freetype2)
FUSE_DESC=	Build grub-mount (require FUSE)

MKFONT_CONFIGURE_ENABLE=	grub-mkfont
MKFONT_LIB_DEPENDS=		libfreetype.so:print/freetype2
MKFONT_BUILD_DEPENDS=		${LOCALBASE}/share/fonts/dejavu/DejaVuSans.ttf:x11-fonts/dejavu

FUSE_CONFIGURE_ENABLE=		grub-mount
FUSE_LIB_DEPENDS=		libfuse.so:sysutils/fusefs-libs

.include <bsd.port.pre.mk>

.if ${ARCH} != amd64
PLIST_SUB+=	EFIEMU="@comment "
.else
PLIST_SUB+=	EFIEMU=""
.endif

post-patch:
	@${LN} -s ${LOCALBASE}/share/fonts/dejavu/DejaVuSans.ttf ${WRKSRC}
	@${TOUCH} -t 200001010000 ${WRKSRC}/Makefile.util.def
	@${REINPLACE_CMD} -e '/[^auU]sed/ s,sed,gsed,g' ${WRKSRC}/util/grub.d/*.in \
		${WRKSRC}/util/*.in ${WRKSRC}/util/i386/efi/grub-dumpdevtree \
		${WRKSRC}/util/bash-completion.d/grub-completion.bash.in

pre-configure:
	cd ${WRKSRC} && ./bootstrap
	sed -i.bak  '23s/.*/typedef uint64_t u64;/' ${WRKSRC}/grub-core/lib/libgcrypt-grub/src/types.h

post-configure:
	@${LN} -sfh /usr/include/machine /usr/include/sys /usr/include/x86 \
		${WRKSRC}/grub-core

.include <bsd.port.post.mk>
