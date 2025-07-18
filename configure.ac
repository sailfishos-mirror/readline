dnl
dnl Configure script for readline library
dnl
dnl report bugs to chet@po.cwru.edu
dnl
dnl Process this file with autoconf to produce a configure script.

# Copyright (C) 1987-2024 Free Software Foundation, Inc.

#   This program is free software: you can redistribute it and/or modify
#   it under the terms of the GNU General Public License as published by
#   the Free Software Foundation, either version 3 of the License, or
#   (at your option) any later version.

#   This program is distributed in the hope that it will be useful,
#   but WITHOUT ANY WARRANTY; without even the implied warranty of
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#   GNU General Public License for more details.

#   You should have received a copy of the GNU General Public License
#   along with this program.  If not, see <http://www.gnu.org/licenses/>.

AC_REVISION([for Readline 8.3, version 2.103])

AC_INIT(readline, 8.3, bug-readline@gnu.org)

dnl make sure we are using a recent autoconf version
AC_PREREQ(2.69)

AC_CONFIG_SRCDIR(readline.h)
AC_CONFIG_AUX_DIR(./support)
AC_CONFIG_HEADERS(config.h)

dnl update the value of RL_READLINE_VERSION in readline.h when this changes
LIBVERSION=8.3

AC_CANONICAL_HOST
AC_CANONICAL_BUILD

dnl configure defaults
opt_curses=no
opt_shared_termcap_lib=no

dnl arguments to configure
AC_ARG_WITH(curses, AS_HELP_STRING([--with-curses], [use the curses library instead of the termcap library]), opt_curses=$withval)
AC_ARG_WITH(shared-termcap-library, AS_HELP_STRING([--with-shared-termcap-library], [link the readline shared library against the termcap/curses shared library [[default=NO]]]), opt_shared_termcap_lib=$withval)

if test "$opt_curses" = "yes"; then
	prefer_curses=yes
fi

dnl option parsing for optional features
opt_multibyte=yes
opt_static_libs=yes
opt_shared_libs=yes
opt_install_examples=yes
opt_bracketed_paste_default=yes

AC_ARG_ENABLE(multibyte, AS_HELP_STRING([--enable-multibyte], [enable multibyte characters if OS supports them]), opt_multibyte=$enableval)
AC_ARG_ENABLE(shared, AS_HELP_STRING([--enable-shared], [build shared libraries [[default=YES]]]), opt_shared_libs=$enableval)
AC_ARG_ENABLE(static, AS_HELP_STRING([--enable-static], [build static libraries [[default=YES]]]), opt_static_libs=$enableval)
AC_ARG_ENABLE(install-examples, AS_HELP_STRING([--disable-install-examples], [don't install examples [[default=install]]]), opt_install_examples=$enableval)

AC_ARG_ENABLE(bracketed-paste-default, AS_HELP_STRING([--disable-bracketed-paste-default], [disable bracketed paste by default [[default=enable]]]), opt_bracketed_paste_default=$enableval)

if test $opt_multibyte = no; then
AC_DEFINE(NO_MULTIBYTE_SUPPORT)
fi

if test $opt_bracketed_paste_default = yes; then
	BRACKETED_PASTE='-DBRACKETED_PASTE_DEFAULT=1'
else
	BRACKETED_PASTE='-DBRACKETED_PASTE_DEFAULT=0'
fi
AC_SUBST(BRACKETED_PASTE)

dnl load up the cross-building cache file -- add more cases and cache
dnl files as necessary

dnl Note that host and target machine are the same, and different than the
dnl build machine.

CROSS_COMPILE=
if test "x$cross_compiling" = "xyes"; then
    case "${host}" in
    *-cygwin*)
        cross_cache=${srcdir}/cross-build/cygwin.cache
        ;;
    *-mingw*)
        cross_cache=${srcdir}/cross-build/mingw.cache
        ;;
    i[[3456]]86-*-beos*)
        cross_cache=${srcdir}/cross-build/x86-beos.cache
        ;;
    *)  echo "configure: cross-compiling for $host is not supported" >&2
        ;;
    esac
    if test -n "${cross_cache}" && test -r "${cross_cache}"; then
        echo "loading cross-build cache file ${cross_cache}"
        . ${cross_cache}
    fi
    unset cross_cache
    CROSS_COMPILE='-DCROSS_COMPILING'
    AC_SUBST(CROSS_COMPILE)
fi

echo ""
echo "Beginning configuration for readline-$LIBVERSION for ${host_cpu}-${host_vendor}-${host_os}"
echo ""

# We want these before the checks, so the checks can modify their values.
test -z "$CFLAGS" && want_auto_cflags=1

AC_PROG_MAKE_SET
AC_PROG_CC
AC_USE_SYSTEM_EXTENSIONS

# If we're using gcc and the user hasn't specified CFLAGS, add -O2 to CFLAGS
if test -n "$want_auto_cflags" ; then
	AUTO_CFLAGS="-g ${GCC:+-O2}"
#	STYLE_CFLAGS="${GCC:+-Wno-parentheses} ${GCC:+-Wno-format-security} ${GCC:+-Wno-tautological-constant-out-of-range-compare}"
	STYLE_CFLAGS="${GCC:+-Wno-parentheses} ${GCC:+-Wno-format-security}"
fi

dnl this macro is obsolete
dnl AC_PROG_GCC_TRADITIONAL
AC_PROG_INSTALL
AC_CHECK_TOOL(AR, ar)
dnl Set default for ARFLAGS, since autoconf does not have a macro for it.
dnl This allows people to set it when running configure or make
test -n "$ARFLAGS" || ARFLAGS="cr"
AC_PROG_RANLIB

AM_PROG_INSTALL_SH

MAKE_SHELL=/bin/sh
AC_SUBST(MAKE_SHELL)

dnl include files for gettext

m4_include([m4/codeset.m4])

AC_C_CONST
AC_C_INLINE
AC_C_PROTOTYPES
AC_C_CHAR_UNSIGNED
AC_C_VOLATILE

AC_TYPE_SIZE_T
AC_CHECK_TYPE(ssize_t, int)

AC_TYPE_MODE_T

AC_HEADER_STAT
AC_HEADER_DIRENT

AC_CHECK_FUNCS(fcntl gettimeofday kill lstat pselect readlink select setitimer)
AC_CHECK_FUNCS(fnmatch memmove putenv setenv setlocale strcasecmp \
		strpbrk sysconf tcgetattr tcgetwinsize tcsetwinsize \
		vsnprintf)
AC_CHECK_FUNCS(isascii isxdigit)
AC_CHECK_FUNCS(getpwent getpwnam getpwuid)

AC_FUNC_CHOWN
AC_FUNC_STRCOLL

AC_CHECK_HEADERS(fcntl.h unistd.h stdlib.h varargs.h stdarg.h stdbool.h \
		string.h strings.h \
		limits.h locale.h pwd.h memory.h termcap.h termios.h termio.h)
AC_CHECK_HEADERS(sys/ioctl.h sys/pte.h sys/stream.h sys/select.h \
		sys/time.h sys/file.h)

AC_CHECK_HEADERS(sys/ptem.h,,,
[[
#if HAVE_SYS_STREAM_H
#  include <sys/stream.h>
#endif
]])

AC_SYS_LARGEFILE

BASH_SYS_SIGNAL_VINTAGE
BASH_SYS_REINSTALL_SIGHANDLERS

BASH_FUNC_POSIX_SETJMP
BASH_FUNC_LSTAT
BASH_FUNC_STRCOLL

BASH_CHECK_GETPW_FUNCS

AC_HEADER_TIOCGWINSZ

BASH_TYPE_SIG_ATOMIC_T

BASH_HAVE_TIOCSTAT
BASH_HAVE_FIONREAD
BASH_CHECK_SPEED_T
BASH_STRUCT_WINSIZE
BASH_STRUCT_DIRENT_D_INO
BASH_STRUCT_DIRENT_D_FILENO
BASH_STRUCT_TIMEVAL

AC_CHECK_HEADERS(libaudit.h)
AC_CHECK_DECLS([AUDIT_USER_TTY],,, [[#include <linux/audit.h>]])

dnl yuck
case "$host_os" in
aix*)   prefer_curses=yes ;;
esac
BASH_CHECK_LIB_TERMCAP
if test "$TERMCAP_LIB" = "./lib/termcap/libtermcap.a"; then
	if test "$prefer_curses" = yes; then
		TERMCAP_LIB=-lcurses
	else
		TERMCAP_LIB=-ltermcap	#default
	fi
fi
# Windows ncurses installation
if test "$TERMCAP_LIB" = "-lncurses"; then
	AC_CHECK_HEADERS(ncurses/termcap.h)
fi
case "$opt_shared_termcap_lib" in
[[Yy]][[Ee]][[Ss]])	SHARED_TERMCAP="$TERMCAP_LIB" ;;
-l*)		SHARED_TERMCAP="$opt_shared_termcap_lib" ;;
esac

case "$TERMCAP_LIB" in
-ltinfo)  TERMCAP_PKG_CONFIG_LIB=tinfo ;;
-lcurses) TERMCAP_PKG_CONFIG_LIB=ncurses ;;
-lncursesw) TERMCAP_PKG_CONFIG_LIB=ncursesw ;;
-lncurses) TERMCAP_PKG_CONFIG_LIB=ncurses ;;
-ltermcap) TERMCAP_PKG_CONFIG_LIB=termcap ;;
*) TERMCAP_PKG_CONFIG_LIB=termcap ;;
esac

BASH_CHECK_MULTIBYTE

case "$host_cpu" in
*cray*)	LOCAL_CFLAGS=-DCRAY ;;
*s390*) LOCAL_CFLAGS=-fsigned-char ;;
esac

case "$host_os" in
isc*)	LOCAL_CFLAGS=-Disc386 ;;
hpux*)	LOCAL_CFLAGS="-DTGETENT_BROKEN -DTGETFLAG_BROKEN" ;;
esac

# shared library configuration section
#
# Shared object configuration section.  These values are generated by
# ${srcdir}/support/shobj-conf
#
if test -f ${srcdir}/support/shobj-conf; then
        AC_MSG_CHECKING(configuration for building shared libraries)
        eval `TERMCAP_LIB=$TERMCAP_LIB ${CONFIG_SHELL-/bin/sh} ${srcdir}/support/shobj-conf -C "${CC}" -c ${host_cpu} -o ${host_os} -v ${host_vendor}`

	# SHARED_TERMCAP is set only if opt_shared_termcap_library is set
	case "$SHLIB_LIBS" in
	*curses*|*tinfo*)	;;
	*termcap*|*termlib*)	;;	# common aliases
	*)		SHLIB_LIBS="$SHLIB_LIBS $SHARED_TERMCAP" ;;
	esac
	
        AC_SUBST(SHOBJ_CC)
        AC_SUBST(SHOBJ_CFLAGS)
        AC_SUBST(SHOBJ_LD)
        AC_SUBST(SHOBJ_LDFLAGS)
	AC_SUBST(SHOBJ_XLDFLAGS)
        AC_SUBST(SHOBJ_LIBS)
        AC_SUBST(SHOBJ_STATUS)
	AC_SUBST(SHLIB_STATUS)
	AC_SUBST(SHLIB_XLDFLAGS)
	AC_SUBST(SHLIB_DOT)
	AC_SUBST(SHLIB_LIBPREF)
	AC_SUBST(SHLIB_LIBSUFF)
	AC_SUBST(SHLIB_LIBVERSION)
	AC_SUBST(SHLIB_DLLVERSION)
	AC_SUBST(SHLIB_LIBS)
        AC_MSG_RESULT($SHLIB_STATUS)

	# SHLIB_STATUS is either `supported' or `unsupported'.  If it's
	# `unsupported', turn off any default shared library building
	if test "$SHLIB_STATUS" = 'unsupported'; then
		opt_shared_libs=no
	fi

	# shared library versioning
	# quoted for m4 so I can use character classes
	SHLIB_MAJOR=[`expr "$LIBVERSION" : '\([0-9]\)\..*'`]
	SHLIB_MINOR=[`expr "$LIBVERSION" : '[0-9]\.\([0-9]\).*'`]
	AC_SUBST(SHLIB_MAJOR)
	AC_SUBST(SHLIB_MINOR)
fi

if test "$opt_static_libs" = "yes"; then
	STATIC_TARGET=static
	STATIC_INSTALL_TARGET=install-static
fi
if test "$opt_shared_libs" = "yes"; then
	SHARED_TARGET=shared
	SHARED_INSTALL_TARGET=install-shared
fi

AC_SUBST(STATIC_TARGET)
AC_SUBST(SHARED_TARGET)
AC_SUBST(STATIC_INSTALL_TARGET)
AC_SUBST(SHARED_INSTALL_TARGET)

if test "$opt_install_examples" = "yes"; then
	EXAMPLES_INSTALL_TARGET=install-examples
fi
AC_SUBST(EXAMPLES_INSTALL_TARGET)

case "$build_os" in
msdosdjgpp*)	BUILD_DIR=`pwd.exe` ;;	# to prevent //d/path/file
*)		BUILD_DIR=`pwd` ;;
esac

case "$BUILD_DIR" in
*\ *)	BUILD_DIR=`echo "$BUILD_DIR" | sed 's: :\\\\ :g'` ;;
*)	;;
esac

AC_SUBST(BUILD_DIR)

# CFLAGS=${CFLAGS-"$AUTO_CFLAGS"}
if test -n "$want_auto_cflags"; then
	CFLAGS="$AUTO_CFLAGS"
fi
CFLAGS="$CFLAGS $STYLE_CFLAGS"

AC_SUBST(CFLAGS)
AC_SUBST(LOCAL_CFLAGS)
AC_SUBST(LOCAL_LDFLAGS)
AC_SUBST(LOCAL_DEFS)
AC_SUBST(STYLE_CFLAGS)

AC_SUBST(AR)
AC_SUBST(ARFLAGS)

AC_SUBST(host_cpu)
AC_SUBST(host_os)

AC_SUBST(LIBVERSION)

AC_SUBST(TERMCAP_LIB)
AC_SUBST(TERMCAP_PKG_CONFIG_LIB)

AC_CONFIG_FILES([Makefile doc/Makefile examples/Makefile shlib/Makefile readline.pc history.pc])

dnl Makefile uses this timestamp file to record whether config.h is up to date.
AC_CONFIG_COMMANDS([stamp-h], [echo > stamp-h])

AC_OUTPUT
