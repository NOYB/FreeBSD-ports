# $FreeBSD$
#   $MCom: ports/finance/gnucash/Makefile,v 1.26 2008/12/31 03:25:18 mezz Exp $

PORTNAME=	gnucash
PORTVERSION=	2.6.16
PORTREVISION=	1
CATEGORIES=	finance gnome
MASTER_SITES=	SF/${PORTNAME}/${PORTNAME}%20%28stable%29/${PORTVERSION}

MAINTAINER=	madpilot@FreeBSD.org
COMMENT=	Quicken-like money and finance manager

LICENSE=	GPLv2 GPLv3
LICENSE_COMB=	dual

LIB_DEPENDS=	libgnome-keyring.so:security/libgnome-keyring \
		libwebkitgtk-1.0.so:www/webkit-gtk2 \
		libgoffice-0.8.so:devel/goffice \
		libfontconfig.so:x11-fonts/fontconfig \
		libfreetype.so:print/freetype2 \
		libsecret-1.so:security/libsecret \
		libsoup-2.4.so:devel/libsoup
RUN_DEPENDS=	p5-libwww>=0:www/p5-libwww \
		p5-Date-Manip>=0:devel/p5-Date-Manip \
		p5-Finance-Quote>=0:finance/p5-Finance-Quote

OPTIONS_DEFINE=	AQBANKING OFX DATABASE LOCALE_TAX PYTHON GTKMM DOCS
OPTIONS_SINGLE=	GV
OPTIONS_SINGLE_GV=	GUILE1 GUILE2
OPTIONS_DEFAULT=	GUILE1

AQBANKING_DESC=	AqBanking support (HBCI/OpenHBCI)
OFX_DESC=	OFX support
DATABASE_DESC=	Enable SQL database backends
LOCALE_TAX_DESC=	Enable localized tax categories
GTKMM_DESC=	Enable GTKMM based UI
GUILE1_DESC=	Use lang/guile
GUILE2_DESC=	Use lang/guile2

OPTIONS_SUB=		yes
AQBANKING_CONFIGURE_ENABLE=aqbanking
AQBANKING_LIB_DEPENDS=	libgwengui-gtk2.so:devel/gwenhywfar-gtk2 \
			libktoblzcheck.so:finance/ktoblzcheck \
			libaqbanking.so:finance/aqbanking
OFX_CONFIGURE_ENABLE=	ofx
OFX_LIB_DEPENDS=	libofx.so:finance/libofx
DATABASE_CONFIGURE_ENABLE=dbi
DATABASE_BUILD_DEPENDS=	libdbi-drivers>=0:databases/libdbi-drivers
DATABASE_LIB_DEPENDS=	libdbi.so:databases/libdbi
DATABASE_RUN_DEPENDS=	libdbi-drivers>=0:databases/libdbi-drivers
DATABASE_CONFIGURE_ON=	--with-dbi-dbd-dir=${LOCALBASE}/lib/dbd
LOCALE_TAX_CONFIGURE_ON=--enable-locale-specific-tax
PYTHON_CONFIGURE_ON=	--enable-python
PYTHON_USES=		python
GTKMM_USE=		GNOME=gtkmm24
GTKMM_CONFIGURE_ON=	--enable-gtkmm
GTKMM_CONFIGURE_OFF=	--disable-gtkmm
GUILE1_LIB_DEPENDS=     libguile.so:lang/guile
GUILE1_BUILD_DEPENDS=	slib-guile>0:lang/slib-guile
GUILE1_RUN_DEPENDS=	slib-guile>0:lang/slib-guile
GUILE2_LIB_DEPENDS=     libguile-2.0.so:lang/guile2
GUILE2_BUILD_DEPENDS=	slib-guile2>0:lang/slib-guile2
GUILE2_RUN_DEPENDS=	slib-guile2>0:lang/slib-guile2

USE_XORG=	x11
USES=		compiler:c++11-lib desktop-file-utils gettext gmake libtool localbase \
		perl5 pkgconfig shebangfix tar:bzip2
INSTALLS_ICONS=	yes
USE_GNOME+=	cairo gtk20 intltool libgnomecanvas libxml2 libxslt
GNU_CONFIGURE=	yes
USE_LDCONFIG=	yes
CONFIGURE_ARGS=	--disable-error-on-warning \
		--disable-doxygen
SHEBANG_FILES=	src/gnc-test-env.pl src/quotes/gnc-fq-dump

GLIB_SCHEMAS=	org.gnucash.dialogs.business.gschema.xml \
		org.gnucash.dialogs.checkprinting.gschema.xml \
		org.gnucash.dialogs.commodities.gschema.xml \
		org.gnucash.dialogs.export.csv.gschema.xml \
		org.gnucash.dialogs.gschema.xml \
		org.gnucash.dialogs.import.csv.gschema.xml \
		org.gnucash.dialogs.import.generic.gschema.xml \
		org.gnucash.dialogs.import.qif.gschema.xml \
		org.gnucash.dialogs.reconcile.gschema.xml \
		org.gnucash.dialogs.sxs.gschema.xml \
		org.gnucash.dialogs.totd.gschema.xml \
		org.gnucash.gschema.xml \
		org.gnucash.history.gschema.xml \
		org.gnucash.warnings.gschema.xml \
		org.gnucash.window.pages.account.tree.gschema.xml \
		org.gnucash.window.pages.gschema.xml

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MAQBANKING}
GLIB_SCHEMAS+=	org.gnucash.dialogs.import.hbci.gschema.xml
.endif

.if ${PORT_OPTIONS:MOFX}
GLIB_SCHEMAS+=	org.gnucash.dialogs.import.ofx.gschema.xml
.endif

.include <bsd.port.pre.mk>

.if ${CHOSEN_COMPILER_TYPE} == gcc
LDFLAGS+=	-lstdc++
.endif

post-install:
.if ${PORT_OPTIONS:MPYTHON}
	@(cd ${STAGEDIR}${PREFIX} \
		&& ${PYTHON_CMD} ${PYTHON_LIBDIR}/compileall.py \
		-d ${PYTHONPREFIX_SITELIBDIR} -f ${PYTHONPREFIX_SITELIBDIR:S;${PREFIX}/;;}/${PORTNAME})
	@(cd ${STAGEDIR}${PREFIX} \
		&& ${PYTHON_CMD} -O ${PYTHON_LIBDIR}/compileall.py \
		-d ${PYTHONPREFIX_SITELIBDIR} -f ${PYTHONPREFIX_SITELIBDIR:S;${PREFIX}/;;}/${PORTNAME})
	@${STRIP_CMD} ${STAGEDIR}${PYTHON_SITELIBDIR}/gnucash/_gnucash_core_c.so
.endif
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/lib*so*
	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/gnucash/lib*.so

.include <bsd.port.post.mk>
